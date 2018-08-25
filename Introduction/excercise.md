# Exercise 1
Is this function pure?
```
const greetUser = (user, greeting = "Hello") => {
  const date = new Date();
  return `${greeting}, ${user}. It is ${date}`;
};
```

# Exercise 2
What is the end result of x?
```
const x = {
  a: 0,
  b: 0,
  c: 0,
};

for (let i=0; i<10; i++) {
  if (i>=0 && i<3) x.a += 1;
  if (i>=4 && i<7) x.b += 2;
  if (i>=8 && i<10) x.c += 3;
}
```

# Exercise 3
Think about how you might refactor Exercise 2 to avoid the side effects
