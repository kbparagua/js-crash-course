# 00: Data Types and Variables

## Basic Data Types
1. `number`
  - Integers and Decimal numbers.
  - Examples: `300`, `0`, `-75`, `3.22`

1. `string`
  - Can be enclosed with single `'` or double `"` quote.
  - Examples: `'Bacon'`, `"Cheese"`, `'Foo "Bar" Baz'`

1. `boolean`
  - Examples: `true`, `false`

## Getting Data Type

Use `typeof` operator to check data type of a variable or an expression. It will return a string indicating the type of the operand.

Syntax: `typeof <operand>`

Examples:
```js
typeof 1000;                  // number
typeof "Hello";               // string
typeof true;                  // boolean
```

## Booleans

Falsy values:
- `false`
- `null`
- `undefined`
- `""` (empty string)
- `0`

Everything else is truthy.

## undefined

- a variable that has no value assigned is of type `undefined`.
- a function returns `undefined` if no value is returned.
- an undeclared variable is of type `undefined`.

Examples:
```js
typeof undefined;             // undefined
typeof undeclaredVariable;    // undefined

var x;
typeof x;                     // undefined

function foo(){
  // return nothing
}
typeof foo();                 // undefined
```

## null

`null` is an object representing null or an "emtpy" value.


## Equality

- `==`
  - simple equality.
  
- `===`
  - strict equality.
  - checks if both values have the same data type.

Examples:
```js
null == undefined // true
null === undefined // false

"" == false // true
"" === false // false

1 == true // true
1 === true // false

[] == false // true
[] === false // false
````
