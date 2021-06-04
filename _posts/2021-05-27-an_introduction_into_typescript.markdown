---
layout: post
title:      "An Introduction Into TypeScript"
date:       2021-05-27 20:12:15 -0400
permalink:  an_introduction_into_typescript
---


​	To understand more about TypeScript lets first understand what it is, a static type checker.  Type checking is the process of checking the data types to insure there are no type errors and that code will behave with the type of data you would expect. Some languages can be pretty relaxed when it comes to changing or manipulating data types, for example in JavaScript you can do operations like: 

```js
let sum = "1" + 2 //=> "12" 
```

With type checking we can make these conversions throw errors to help developers avoid errors and unintentional outcomes like the above example we might expect the sum to be `3` or an error preventing an outcome where the number `2` would be converted to String and vise-versa.

​	When do these type conversions and checks happen? Well, that’s where the static part comes in. The two prominent types of type checking are static and dynamic. Static type checks occur at compile time, when the code is translated to machine code by a compiler (in this case [Babel](https://babeljs.io/)). Dynamic type checks occur at run time, after the code has been compiled and is then executed or run indefinitely.

​	[TypeScript](https://www.typescriptlang.org/) is a static type checker that was built on Javascript, or more specifically a typed superset of JavaScript, that adds additional rules and safety to JavaScript code that can give you a more confident and secure application. TypeScript gets compiled into JavaScript with a custom Babel plugin much like the way React.js or other JavaScript libraries and frameworks compile their code, see also the [JIT compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation) for an in-depth look at JavaScript compilation and the [V8 Engine](https://v8.dev/). 

> TypeScript’s type checker is designed to allow correct programs through while still catching as many common errors as possible. -[TSLang.org](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

I also preserves the standard JavaScript runtime and is guaranteed to run the same way even if you move JavaScript code directly into a TypeScript file.

​	TypeScript is very powerful and has dramatically risen in popularity. It can help you make cleaner, well tested, less error prone code. Having a similar syntax to JavaScript, It has a lower learning curve if you already know JavaScript well and you can translate everything you know about JavaScript into programming in TypeScript.

[TS From Scratch - TSL.org](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

[TypeScript - Homepage](https://www.typescriptlang.org/)

[Type Checking - The Code Boss](https://thecodeboss.dev/2015/11/programming-concepts-static-vs-dynamic-type-checking/)

[Babel - homepage](https://babeljs.io/)

[Program Lifecyle - Wiki](https://en.wikipedia.org/wiki/Program_lifecycle_phase)

[JIT - Wiki](https://en.wikipedia.org/wiki/Just-in-time_compilation)

[V8 Engine](https://v8.dev/)


