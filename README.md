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
function shippingRate(size, weight, speed){
  return ((size + 1) * weight) + speed
}
```

```js
// z is an indirect input
function shippingRate(size, weight, speed){
  return ((size + 1) * weight) + speed + z
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
