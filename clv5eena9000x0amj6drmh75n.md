---
title: "Understanding Pure and Impure Functions in JavaScript"
seoTitle: "Understanding Pure and Impure Functions in JavaScript"
seoDescription: "Functions are fundamental building blocks that can be categorized into two types: pure and impure functions."
datePublished: 2024-04-18T15:30:27.297Z
cuid: clv5eena9000x0amj6drmh75n
slug: pure-and-impure-functions-in-js
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713454131479/83d10087-96cb-4653-9d1c-8837046997ea.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1713454144669/54898101-5baa-4c93-8093-8c3f03c09c25.png
tags: functions, javascript, functional-programming, clean-code

---

Understanding the difference between these two is crucial for developers, as it can greatly influence the code's predictability, testability, and maintainability. This blog post will delve into the concept of pure and impure functions in JavaScript, providing a clear understanding of how they work.

## What is a Pure Function?

A pure function is a function in programming where the return value is solely determined by its input values, without any observable side effects. This means that the output will always be the same for a given input.

### Characteristics of Pure Functions:

1. The function always returns the same result if the same arguments are passed in. It does not depend on any state, or data, change during a program’s execution. It must only depend on its input arguments.
    
2. The function produces no observable side effects such as network requests, input and output devices, or data mutation.
    

### Code example

```javascript
function add(a, b) {
  return a + b;
}
```

In the above example, the function add is a pure function because for any given pair of inputs, it will always return the same output.

## What is an Impure Function?

An impure function is the opposite of a pure function. It is a function that mutates variables outside of its scope, produces side effects, or relies on external mutable state.

### Characteristics of Impure Functions:

1. The function may change external variables or states.
    
2. The function may produce different results for the same input due to interactions with external states or variables.
    

### Code example

```javascript
let num = 2;

function multiply(a) {
  return a * num;
}
```

In the above example, the function multiply is impure. It modifies the global variable num, and the output value will vary based on the state of num.

## Why Does the Distinction Matter?

The fundamental difference between pure and impure functions lies in their predictability. Pure functions are deterministic, meaning for given inputs, the output will always be the same, regardless of what's happening in the rest of the program. This makes them easier to reason about, test, and debug.

In contrast, impure functions can be harder to manage as they depend on and modify external state. However, it's important to note that impure functions are not 'bad'. In fact, they are necessary for most programming tasks, such as interacting with APIs, manipulating the DOM, or handling user input in a web application.

## Conclusion

In conclusion, understanding the difference between pure and impure functions in JavaScript can help developers write more predictable, maintainable, and testable code. While pure functions increase predictability due to their lack of side effects, impure functions are necessary for tasks that need to interact with the outside world. As a best practice, aim to keep the core logic of your application pure and confine side effects to the edges of your program.