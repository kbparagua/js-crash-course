# 00: Data Types and Variables

## Data Types

### Basic
1. `number`
  - Integers and Decimal numbers.
  - Examples: `300`, `0`, `-75`, `3.22`

1. `string`
  - Enclosed with single `'` or double `"` quotes.
  - Examples: `'Bacon'`, `"Cheese"`, `'Foo "Bar" Baz'`

1. `boolean`
  - `true` or `false`.

### Querying Data Type

Use `typeof` operator to get the data type of a variable or an expression. It will return a string indicating the data type of the operand.

Syntax: `typeof <operand>`

Examples:
```js
typeof 1000;    // number
typeof "Hello"; // string
typeof true;    // boolean
```

### Boolean

Falsy values:
- `""` (empty string)
- `NaN`
- `0`
- `false`
- `undefined`
- `null`

"**Empty string** is **Not a Number**. **Zero** is **FUN**"

Everything else is truthy.

Examples:
```js
if (null) alert("null");                  // will NOT alert
if (0) alert(0);                          // will NOT alert

if ("Hello World") alert("Hello World");  // will alert
if (42) alert(42);                        // will alert
```

### NaN
Not a Number.
- returned when a Math function failed.

`NaN` is not equal to itself. Use `isNan()` to check if value is `NaN`.
```js
NaN == NaN // false
isNaN(NaN) // true
```

### undefined
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

### null
`null` is an object representing null or an "emtpy" value.


### Equality
- `==`
  - loosely equal.
  
- `===`
  - strictly equal.
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

## Variables

### Declaration
- with `var`
  - local variable.
  - exists only within the current scope.
  - Example: `var myLocalVariable = 3;`
  
- without `var`
  - global variable.
  - Example: `myGlobalVariable = 5;`

### Scopes

There is no block scope.
```js
if (x > 1){
  var foo = "Hello";
}

console.log(foo); // Hello (foo is still accessible outside the if block)
```

Function is the only scope mechanism available.
```js
function bar(){
  var foo = "Hello";
}

console.log(foo); // ReferenceError: foo is not defined.
```

Local variables declared inside a function will only be accessible inside that function.

### Multiple Declarations

You can delcare multiple variables at once by separating each variable with comma(`,`).

```js
var x = 1, y = 2, z = 3;
var foo, bar = "Hello", baz = 100;
```

### Hoisting
- Variable declarations are processed before any code is executed.
- Declaring a variable anywhere is equivalent to declaring it at the top.
- Only the declaration will be processed on the initial reading and not the initialization or assignment part.

Example:
```js
function foo(){
  var a = 1;
  // some statement here
  var b = 2;
  // another statement here
  var c = 3;
}
```

The above code will be interpreted like this:
```js
function foo(){
  var a, b, c;
  a = 1;
  // some statement here
  b = 2;
  // another statement here
  c = 3;
}
```

[source](http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092)
