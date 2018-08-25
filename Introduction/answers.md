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
const x = new Array(10).fill('').reduce((result, _, i) => {
  if (i>=0 && i<3) result.a += 1
  if (i>=4 && i<7) result.b += 2;
  if (i>=8 && i<10) result.c += 3;
  return result;
}, {
  a: 0,
  b: 0,
  c: 0
});
```
