# Introduction

## What
Functional Programming (FP) is a programming paradigm focused on solving problems in a declaritive way. Let me first start by stating that declaritive programming is nothing more than stating WHAT when programming, rather than HOW. What I mean to say  is FP doesn't much care of how something is achieved, it is more concerned with what is being achieved. This is accomplished by creating pure functions. These functions have an input, they act upon the input, and return a predictable output.

Compare these 2 approaches.

```
let value = 0;
const add = (qty) => { 
  value += qty;
}
add(2);
/* value is initialized to 0, and set to 2 */

const fpAdd = (qty, v) => {
  return qty + v;
};
let value = 0;
value = fpAdd(2, value);
/* value is initialized to 0, the sum is assigned back to value.
```

This is a very basic example but it illustrates the difference between a function which is aware of code outside it's scope, vs. a function which is only aware of it's scope. This is the guiding principle of FP.

## Why

## When
