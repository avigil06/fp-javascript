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
/* value is initialized to 0, and mutated to 2 */

const fpAdd = (v, qty) => {
  return qty + v;
};
let value = 0;
value = fpAdd(value, 2);
/* value is initialized to 0, the sum is assigned back to value.
```

This is a very basic example but it illustrates the difference between a function which is aware of code outside it's scope, vs. a function which is only aware of it's scope. This is the guiding principle of FP. Something that is much more common might be the use of loops and side effects. Loops, by nature are imperitive, as they have no real scoping of there own.

```
/* Get the highest number */
const foo = [5, 3, 6, 2, 7, 9, 10, 1, -10, 4];


/* Loop approach */
let bar = 0;
for (let i=0; i<foo.length; i++) {
  if (foo[i] > bar) {
    bar = foo[i];
   }
} // result: 10;

/* Reduce approach */
const getMax = (result, pred) => pred > result ? pred : result;
bar = foo.reduce(getMax, 0);
```

I know what you are thinking. You're thinking "Andrew, there are still better ways. I could solve that last problem with Math.max". You're absolutely right. You sure can. How about this one?

```
const foo = [
  { id: 1, sort: 10 },
  { id: 2, sort: 3 },
  { id: 3, sort: -10 },
  { id: 4, sort: 50 },
  { id: 5, sort: 5 },
];

/* We can still use the same loop and switch our condition from
*  if (foo[i].sort > bar.sort)
*  ---------------------------
*  Let's try the reduce
*/

const getMax = (result, pred) => pred.sort > result.sort ? pred : result;
const bar = foo.reduce(getMax, { sort: 0 }); // result { id: 4, sort: 50 };
```

## Why
So given the above examples. It's easy to see why using pure functions are helpful. It is easy to reason about a function. Here are a few more reasons to stop mutating state and avoiding mutations.

+ Easy to unit test
+ Swap out implementations and maintain declarative nature
+ A/B test implementations for performance optimizations

## Summary
For a given input, a function will always return the same response.
No Side Effects.
No State Mutations.

## Other Take Aways
I would like to introduce you to one of my favorite JS libraries.

[RamdaJS](https://ramdajs.com/)

## What's next
Now that we have a handle on the corner stone of functional programming, we will introduce function composition in our next section.

