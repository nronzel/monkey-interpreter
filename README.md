## Overview

An interpreter for a fictional programming language called "Monkey". Made while following
along with [Writing An Interpreter In Go by Thorsten Ball](https://interpreterbook.com/)
in an effort to learn more about how interpreted languages work.

## Features

- Variable bindings
- Prefix & Infix operators
- First-class & higher-order functions
- Closures
- Integers
- Booleans
- Arrays
- Hashes
- Builtin functions (`len`, `puts`, array: `first`, `last`, `rest`, `push`)

## Usage

Clone the repository:

```bash
git clone
```

Run the REPL:

```bash
go run .
```

## About

> Excerpts from: [link](https://interpreterbook.com/#the-monkey-programming-language)

What Monkey looks like:

```js
// Bind values to names with let-statements
let version = 1;
let name = "Monkey programming language";
let myArray = [1, 2, 3, 4, 5];
let coolBooleanLiteral = true;

// Use expressions to produce values
let awesomeValue = (10 / 2) * 5 + 30;
let arrayWithValues = [1 + 1, 2 * 2, 3];
```

Monkey supports function literals:

```js
// Define a `fibonacci` function
let fibonacci = fn(x) {
  if (x == 0) {
    0                // Monkey supports implicit returning of values
  } else {
    if (x == 1) {
      return 1;      // ... and explicit return statements
    } else {
      fibonacci(x - 1) + fibonacci(x - 2); // Recursion! Yay!
    }
  }
};
```

Supported data types:

- Booleans
- Strings
- Hashes
- Integers
- Arrays

```js
// Here is an array containing two hashes, that use strings as keys and integers
// and strings as values
let people = [{"name": "Anna", "age": 24}, {"name": "Bob", "age": 99}];

// Getting elements out of the data types is also supported.
// Here is how we can access array elements by using index expressions:
fibonacci(myArray[4]);
// => 5

// We can also access hash elements with index expressions:
let getName = fn(person) { person["name"]; };

// And here we access array elements and call a function with the element as
// argument:
getName(people[0]); // => "Anna"
getName(people[1]); // => "Bob"
```

Functions are first-class citizens and are treated like any other value, allowing
for higher-order functions, and passing functions as values.

```js
// Define the higher-order function `map`, that calls the given function `f`
// on each element in `arr` and returns an array of the produced values.
let map = fn(arr, f) {
  let iter = fn(arr, accumulated) {
    if (len(arr) == 0) {
      accumulated
    } else {
      iter(rest(arr), push(accumulated, f(first(arr))));
    }
  };

  iter(arr, []);
};

// Now let's take the `people` array and the `getName` function from above and
// use them with `map`.
map(people, getName); // => ["Anna", "Bob"]
```

This also allows for closures:

```js
// newGreeter returns a new function, that greets a `name` with the given
// `greeting`.
let newGreeter = fn(greeting) {
  // `puts` is a built-in function we add to the interpreter
  return fn(name) { puts(greeting + " " + name); }
};

// `hello` is a greeter function that says "Hello"
let hello = newGreeter("Hello");

// Calling it outputs the greeting:
hello("dear, future Reader!"); // => Hello dear, future Reader!
```
