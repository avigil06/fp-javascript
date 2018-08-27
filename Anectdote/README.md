# How I started
The moment I started reading about functional programming, I was hooked. I had already waded through enough jQuery soup and
messed with React, Angular, and Vue before deciding I needed a better way to track application states. Side effects where resulting
in unintended states that needed to have exceptional logic to manage.

Jump ahead of some ReactJS projects and I was reading everything I could to get a handle on application state. I came across RamdaJS.
Some of the things I struggled with where testing for object properties in a graceful way. RamdaJS provides functions to help manage
the response of a function in the abscence of a property. Ramda's `prop`, `path`, and `defaultTo` functions are great for providing
reliable function output.

prop allows you to return the value of a property, based on the key name.
```
const test = { id: 1 };
console.log(prop('id', test)); // 1
console.log(prop('error', test)); // undefined
```

path allows you to test a path to a nested property of an object
```
const test = {
  foo: {
    a: 0,
    b: 1
  }
};
console.log(path(['foo', 'a'], test)); // 0
console.log(path(['foo', 'c'], test)); // undefined
console.log(path(['bar', 'a'], test)); // undefined
```

In comparison, if you were to just use a reference, an error exception will be thrown and your application will hard fail.

If you have a function that needs a specific type of output or a default value, you can combine defaultTo with path or prop.
```
const defaultToNull = defaultTo(null);
const test = { id: 1 };
console.log(defaultTo(prop('id', test))); // 1
console.log(defaultTo(prop('error', test))); // null
```

# Where I am now?
After spending time with some of the more utility list functions, I became brave enough to understand pipe and compose. I prefer
the readability of compose as it's easier for me to read a function, with predicate argument, from right to left. Function composition,
has allowed me to leverage single purpose functions in ways that I have not been able to in the past. Suddenly immutable business logic
was accessible to me. Taking small pieces I have already written and using it in more complex functions allows me to stop thinking about the how,
and focus more on the what. If a piece is not working, unit tests around smaller units of calculation allow me to quickly identify the 
bugs and fix is everywhere at once instead of tracking down code and pasting in the bug fix.

My approach has been and continues to be to start at the first data transform. If I need to take a list and widdle it down, to a single
value, I know it will probably be a reducer instead of a filter. If I need to take that final result and transform it for a presentational piece,
I know that I will have a function that takes the result of the reduce, and transforms it. If I start with the final transformation, I will
usually end up writing an overly complicated function that will need to be refactored. Single Purpose functions really do make for cleaner code.

# What is next?
I would like to better understand function programming terminology and other core concepts. There are paradigms inside of FP that present solution scaffolds to problems based on the type of input and output requirements of a solution. I want to explore these next.

# Final Thoughts
In the end, I don't think Functional Programming slows my workflow. I believe I pick up a lot of velocity on the tail end of development by
eliminating unintended application states do to treating data as immutable. I don't always write unit tests but single purpose pure functions
create a perfect environment for me to write a Unit Test. Remember, pure functions state that given the same input, a predictable output will return.
Single purpose functions state I am not doing to much and it is easier to reason about my code. If you pair these principles with variable names
that explain what you are trying to achieve, your code should also require very little additional code comments as it is already very purposeful,
and transparent.
