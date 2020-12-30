# Functional Programming in JavaScript

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

### Equational Reasoning

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
