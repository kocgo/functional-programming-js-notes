# Functional Programming in JavaScript

## Glossary

### Functors
In mathematics, specifically category theory, a functor is a mapping between categories.
According to Haskell and the Fantasy Land specification, a functor is simply something that can be mapped over. In OOP-speak, we’d call it a ‘Mappable’ instead.
If and only if a functor of class always return the same class as itself, this functor is an endofunctor.

Requirements:
- Should transform content (Category A -> Category B)
- .map should return same structure (Object.map returns Object, Array.map returns Array)
- Should return a Functor

Use cases:
- Promises
- Streams
- Trees

### Category

## Imperative vs Declarative Programming

### Imperative

Programs as statements that directly change computed state

- Future reader has to go through all the code to understand it.
- Performant
- What to do
- C, Java

### Declarative

A style of building the structure and elements of computer programs—that expresses the logic of a computation without describing its control flow.

- How to do
- SQL, Regex

## Functions vs Procedures

### Function

Set of instructions takes some input and returns a value.

### Procedure

Set of instructions just executes commands.

## Side Effects

- Pure functions should not effect any other scope

- Pure functions should have direct input & direct output relation:

```js
// All direct inputs
function shippingRate(size, weight, speed) {
  return (size + 1) * weight + speed;
}
```

```js
// z is an indirect input
function shippingRate(size, weight, speed) {
  return (size + 1) * weight + speed + z;
}
```

List of side effects:

- Any I/O (console, files, etc)
- Database Storage
- Network Calls
- DOM
- Timestamps
- Random Numbers
- CPU Heat
- CPU Time Delay

`The goal is to minimize the side-effects rather than removing all.`

## Function Arguments

Unary

```js
function increment(x) {
  return sum(x, 1);
}
```

Binary

```js
function sum(x, y) {
  return x + y;
}
```

## Higher-Order Function

A function that recieves functions as arguments and returns a function.

## Adapters

Flip & Reverse

```js
// Returns a new function with first two args reversed
function flip(fn) {
  return function flipped(arg1, arg2, ...args) {
    return fn(arg2, arg1, ...args);
  };
}

function f(...args) {
  return args;
}

const g = flip(f);

g(1, 2, 3, 4); // [4,3,2,1]
```

spreadArgs ( AKA apply )

```js
function spreadArgs(fn) {
  return function spread(args) {
    return fn(...args);
  };
}

function f(x, y, z, w) {
  return x + y + z + w;
}

const g = spreadArgs(f);

g([1, 2, 3, 4]); // 10
```

not (aka negate)

```js
function not(fn) {
  return fucntion negated(...args){
    return !fn(...args);
  };
}

function isOdd(v){
  return v % 2 === 1;
}

const isEven = not(isOdd);

isEven(4); // true
```

when

```js
function when(fn) {
  return function (predicate) {
    return function (...args) {
      if (predicate(...args)) {
        return fn(...args);
      }
    };
  };
}

when(output)(isShortEnough)(msg1);

// Additional point-free printIf

const printIf = when(output);

printIf(isShortEnough)(msg);
```

### Equational Reasoning ( point-free )

If two functions have the same shape (inputs), they are interchangable.

```js
getPerson((person) => {
  return renderPerson(person);
});
```

Alternative

```js
getPerson(renderPerson);
```

### Composition (Advanced Point-Free)

Composition is taking one functions output and making it another function's input.

```js
function mod(y) {
  return function forX(x) {
    return x % y;
  };
}

function eq(y) {
  return function forX(x) {
    return x === y;
  };
}

function compose(fn2, fn1) {
  return function composed(v) {
    return fn2(fn1(v));
  };
}

/* 
  Execution order is important in functional programming! 
  Compositions will execute from right to left.  
*/

const isOdd = compose(eq(1), mod(2)); // returns function

isOdd(3); // True
```

### Closure

Closure is when a function `remembers` the variables around it even when that function is executed elsewhere.

Any variables you access outside of function.

```js
function makeCounter() {
  let counter = 0;
  return function increment() {
    return ++counter;
  };
}

var c = makeCounter();
c(); // 1
c(); // 2
c(); // 3
```

This function is not a `pure` function because it is not returning same outputs on each call. (same input => same output rule)

String Builder Exercise (on each call a new closure is created, this is the way to keep purity)

```js
function stringBuilder(str) {
  return function next(v) {
    if (typeof v == "string") {
      return stringBuilder(str + v); // Recursion
    }

    return str;
  };
}

stringBuilder("Hello ")("Gokhan")(); // Hello Gokhan
```

### Piping vs Composition
Compose is right-to-left, Pipe is left-to-right.



Pipe
```js
function pipe(...fns) {  
  return function piped(v){  // Returns a function, no execution yet! 
    for (let fn of fns){
      v = fn(v); // Take the input, and start executing, re-assign 'v' each time
    }
    
    return v;
  }
}
```

Compose
```js
function compose(...fns) {
  return pipe(...fns.reverse()); // Just reverse the order of functions
}
```

### Immutability

Immutability is a concept of controlling the mutation.

- Avoid making variable assignments as possible???



