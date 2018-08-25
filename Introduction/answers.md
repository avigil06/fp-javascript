# Exercise 1
A pure function can only use local state to produce an outcome. By using `new Date()` we have dirtied up our pure function.
We could refactor greetUser with a third argument to remove the date `const` from inside the function body.

# Exercise 2
```
const x = {
  a: 3,
  b: 6,
  c: 6,
};
```

# Exercise 3
This is just one solution
```
const addOne = (value) => value + 1;
const addTwo = (value) => value + 2;
const addThree = (value) => value + 3;

const range = new Array(10).fill('');
const x = range.reduce((result, _, i) => {
  if (i>=0 && i<3) result.a = addOne(result.a);
  if (i>=4 && i<7) result.b = addTwo(result.b);
  if (i>=8 && i<10) result.c = addThree(result.c);
  return result;
}, { a: 0, b: 0, c: 0 });

// It was unnecessary to write functions to add for the different scenarios,
// I just think it clarifies the intention of the scenarios.
```
