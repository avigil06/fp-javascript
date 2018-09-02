# Functional Programming (FP): Currying & Composition

## Premise
My Premise is that sprinkling some functional practices into your source code can help to eliminate bugs, 
make your code more testable, and allow others to maintain code you have written.

FP has a rather high bar of entry. We are mostly taught, through education and example, to code in an imperative 
way. The question we are always trying to answer is the “how”. How do we solve a problem. How does `x` become 5.

FP is declarative by design. We are not concerned with the how. We are always asking ourselves about the what. 
Instead of asking how does `x` become `5`, we are concerned with what `x` should be. Should `x` even be 5?

By focusing on the “What” of a problem, we can break each question into micro questions.

## Goals
So after this introduction to function currying and composition, we should have a better understanding of FP in 
general, we should have extracted some logic into easily tested function units, and we will have made our sample 
project more readable and maintainable.

+ Easy testing
+ Maintainable
+ Lower cognitive load

That last one is probably a tall order to FP but it is important to ensure even developers without a background 
in FP are able to read our project and understand what it is trying to achieve.

## The project
```
let students = [
  { name: 'Sheldon', grade: 100, gender: 'M' },
  { name: 'Lenard', grade: 95, gender: 'M' },
  { name: 'Raj', grade: 89, gender: 'M' },
  { name: 'Howard', grade: 91, gender: 'M' },
  { name: 'Stuart', grade: 64, gender: 'M' },
  { name: 'Penny', grade: 64, gender: 'F' },
  { name: 'Bernadette', grade: 88, gender: 'F' },
  { name: 'Zach', grade: 43, gender: 'F' },
];

const sortStudents = (a, b) => {
  if (a.level > b.level) return 1;
  if (b.level > a.level) return -1;
  if (a.grade > b.grade) return 1;
  if (b.grade > a.grade) return -1;
  return 0;
}

const compose = (...fns) => fns.reduce((f, g) => (...args) => f(g(...args)))
```

### Project Tasks
1) Start all students at level 1
2) If a users grade >= 65, increase that students level
3) Separate all students by gender
3) Sort the male and female lists first by level, then by grade

For brevity I gave the sortStudents function as it will work both in the imperative and declarative approaches

## Imperative Approach
```
const maleStudents = [];
const femaleStudents = [];

for (let i=0; i<students.length; i++) {
  // attach level to each student
  students[i].level = 1;

  // should they go up a level
  if (students[i].grade >= 65) {
    students[i].level++;
  }

  // distribute male and female students
  if (students[i].gender === 'M') {
    maleStudents.push(students[i]);
  } else {
    femaleStudents.push(students[i]);
  }
}

const sortedMaleStudents = maleStudents.sort(sortStudents);
const sortedFemaleStudents = femaleStudents.sort(sortStudents);
```

This approach works fine. We have achieved all of our goals. Some may disagree with me but I find this approach 
difficult to follow the "What" for what we are wanting to achieve. It does describe how it is trying to achieve 
it, but would you be able to figure out what exactly the purpose of this for loop is without the singe line 
comments? Lets try to refactor this using function currying and composition.

## Currying
Currying is the process of taking a functions signature and distributing it amongst multiple functions so each 
one only has a single argument.

```
const sum = (a, b) => a + b;
const currySum = (a) => (b) => a + b;
```

In the above example, we can see sum takes 2 arguments. It needs both of these arguments or it will fail. So why 
would `currySum` be anymore useful.

```
const addFive = currySum(5);
console.log(addFive(2)) // 7
```
With `currySum` we can create additional functions. This introduces an important part of FP, Partial Completion. 
We know functions will `Run to Completion`. In the case of a curried function, our completion results in a callable 
function to do more things. For the example above, it means we are waiting for another argument to be passed so we 
can complete our `a + b` formula.

## Composition
Function Composition is simply using 2 or more functions to create a function of higher complexity

```
const toString = val => val.toString();
const addFiveAndToString = compose(toString, addFive);
console.log(addFiveAndToString(2)) // "7"
```

I have already provided the compose function from the original project source. Traditionally, compose executes from 
right to left.

## Declarative Approach
```
const startAtLevelOne = (student) => ({ ...student, level: 1 });
const isMale = (student) => student.gender === 'M';
const hasPassingGrade = (student) => student.grade >= 65;
const addOne = currySum(1);
const promoteLevel = (student) => ({ ...student, level: addOne(student.level) });
const promoteLevelIfHasPassingGrade = (student) => hasPassingGrade(student) ? promoteLevel(student) : student;

const maleStudents = students
  .map(startAtLevelOne)
  .filter(isMale)
  .map(promoteLevelIfHasPassingGrade)
  .sort(sortStudents);

const femaleStudents = maleStudents
  .map(startAtLevelOne)
  .filter((student) => !isMale(student))
  .map(promoteLevelIfHasPassingGrade)
  .sort(sortStudents);
```

If you are familiar with the array methods such as map, filter, sort, and reduce, you will note that we are doing 
many loops now instead of just a single loop. This approach is slightly less performant than the imperative approach. 
You will also note that not a single code comment was used. We also removed all of the business logic into single 
purpose, pure functions. This makes all of our function units testable at the most granular level.

Lets look closer at our refactor. Did we achieve our goals?

#### Easy Testing
We wanted to make our solution more unit testable. Since we have extracted all of the business logic into pure 
functions, we know that we can pass in a specific input, and assert a predictable output. This goal is complete.

#### Maintainable
Again, since all of our business logic is answering a specific micro question, we are able to debug, revise, even 
replace each of these micro solutions with another solution, making it easier to maintain and optimize our application.

#### Lower Cognitive Load
We did not use any code comments in our refactor, but because of the single purpose nature of each of our micro 
solutions, it is easy to see what `maleStudents` and `femaleStudents` is made up of. We don't need to read the business 
logic to know what it is we are trying to do. In fact, unless we are debugging this application, we have no reason to 
care how these micro solutions are executing.

# Can We Go Further?
It is difficult enough to jump into functional programming on your own. I want to introduce you to a library that I use 
extensively to make the transition easier.

RamdaJS is a collection of utilities which allow you to act upon different types of data to create complex solutions. 
Lets do one more refactor.

```
import { compose, map, filter, sort, add } from 'ramda';
const startAtLevelOne = (student) => ({ ...student, level: 1 });
const isGender = (gender) => (student) => student.gender === gender;
const hasPassingGrade = (student) => student.grade >= 65;
const addOne = add(1);
const promoteLevel = (student) => ({ ...student, level: addOne(student.level) });
const promoteLevelIfHasPassingGrade = (student) => hasPassingGrade(student) ? promoteLevel(student) : student;

const getStudentsOfGender = (gender) => compose(
    sort(sortStudents),
    map(promoteLevelIfHasPassingGrade),
    filter(isGender(gender)),
    map(startAtLevelOne),
);

const maleStudents = getStudentsOfGender('M')(students);
const femaleStudents = getStudentsOfGender('F')(students);
``` 


