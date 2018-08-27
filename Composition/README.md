# What is Function Composition?
Imagine your functions are single purpose. How do would you take a list of users, filter by age, and get a list of names?
Sure, you could loop through your dataset, but what if you need to use this same subset in multiple places. The answer is to use Function Composition.
This means that the result of one function is the arguments for another. This allows you to compose complex logic out of small, maintainable,
and testable, units of logic.

```
// Our list of people
const people = [
 { name: 'John Adams', age: 18 },
 { name: 'Ameilia Earheart', age: 45 },
 { name: 'George Washington', age: 105 },
 { name: 'Orville Wright', age: 42 }
];

// Uncomposed Solution
let people45Plus = [];
for (i in people) {
  const person = people[i];
  if (person.age >= 45) {
    people45Plus.push(person.name);
  }
}
```

We have a list of people, we want to get all people age 45 and above, but we only want their names.
```
const filterAge45Plus = ({ age }) => age >= 45;
const getName = ({ name }) => name;
```

These are our 2 functions. We are going to use RamdaJS to help us wire this up. We want the filter, map, and compose functions.
```
import { filter, map, compose } from 'ramda';

// filter  - a functional filter since we can't use our array method
// map     - a functional map since we can't use our array method
// compose - executes function arguments from right to left

const getNameOfAge45Plus = compose(
  map(getName),
  filter(filterAge45Plus),
)

const people45Plus = getNameOfAge45Plus(people);
```

In this scenario, the composed function does not seem like a big deal. The uncomposed solution looks simple enough,
but what if we add a third and fourth property to our people list objects. If we add gender as a field, and a boolean field of isPresident,
that significantly increases our complexity. Let's see how easy it would be to get two additional lists. All male gender people, and
all people who have been president of the USA.

```
// Our list of people
const people = [
 { name: 'John Adams', age: 18, isMale: true, isPresident: true },
 { name: 'Ameilia Earheart', age: 45, isMale: false, isPresident: false },
 { name: 'George Washington', age: 105, isMale: true, isPresident: true },
 { name: 'Orville Wright', age: 42, isMale: true, isPresident: false }
];

const isPresident = ({ isPresident }) => isPresident;
const isMale = ({ isMale }) => isMale;

const getNameOfPresidents = compose(
  map(getName),
  filter(isPresident)
);
const getNameOfMen = compose(
  map(getName),
  filter(isPresident)
);

const presidents = getNameOfPresidents(people);
const men = getNameOfMen(people);
```

That was much easier than adding additional branching logic to our original loop or setting up additional empty lists to append to.
