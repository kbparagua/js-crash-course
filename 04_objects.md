# 04: Objects

## Creating Basic Object

Object is a collection of properties, wherein each property is composed of a key-value pair.

```js
var bruce = {"firstname": "Bruce", "lastname": "Lee"};
```

### Keys

Keys are automatically converted to strings, so we can ommit the quotes around the keys.

```js
var bruce = {firstname: "Bruce", lastname: "Lee"};
```

If quotes are ommited, a key should be a valid identifier name.

```js
// This will not work.
var obj = {4x4: "four by four"}; // Syntax Error

// Must enclose with quotes.
var obj = {"4x4": "four by four"};
```

If a non-string key is provided, such as a number or a boolean, it will be typecasted to string.

```js
var object = {false: "false value", 100: "one hundred"};

// The object above is equivalent to this object:
{"false": "false value", "100": "one hundred"};
```

### Values

Property values can be any expressions.

```js
var bruce = {
  firstname: "Bruce",
  lastname: "Lee",
  age: 75,
  alive: false,
  family: {wife: "Linda", son: "Brandon", daughter: "Shannon"},
  kick: function(){ alert("watta!"); }
};
```

As you can see, we can even use another object as a property value. Basically, anything that can be assigned to a variable can also be a property value.

## Accessing Properties

We use the dot operator (`.`) to access properties. 

```js
alert(bruce.lastname); // "Lee"
```

Use the same operator to manipulate or add properties.

```js
// Change property value.
bruce.age = 76;

// Add new property.
bruce.lastMovie = "Game of Death";
```

### Irregular Keys

Dot operator can only access properties with keys that qualify as valid identifier name.

```js
var obj = {"4x4": "Four by Four"};
alert(obj.4x4) // Syntax Error
```

Use Bracket notation (`[]`) to access irregular keys.

```js
var obj = {"4x4": "Four by Four"};
alert(obj["4x4"]); // "Four by Four"
```

### Using Expression to Access Property

Use Bracket notation to access a property using a variable or any expressions.

```js
var obj = {
  foo: "Foo", 
  bar: "Bar"
};

var key = "bar";

// Do not use dot operator!
alert(obj.key); // undefined

// Use bracket notation to access using variable/expression
alert(obj[key]); // "Bar"

// Formatted for readability.
alert(
  obj[
    (function(){ return "foo"; })()
  ];
); // "Foo"
```

If the passed expression does not result to a string, it will be typecasted to a string.

```js
var obj = {"100": "One Hundred"};

alert(obj[50 + 50]); // "One Hundred"
```

## Methods

Method is just a name used as a convention to refer to a property that has a function as its value.
Basically, a method is a property that can be invoked.

```js
var obj = {
  myMethod: function(){ alert("I am a method!"); }
};

// Invoke method.
obj.myMethod(); // "I am a method!"
```

### This Object

`this` is a special variable available inside a method, that refers to the object that invoked it.

```js
var spongebob = {
  name: "Spongebob Squarepants",
  sayHi: function(){
    alert("Hi! I am " + this.name);
  };
};

spongebob.sayHi(); // "Hi! I am Spongebob Squarepants"
```

The value of `this` is evaluated at run-time, so it will always be equal to the object invoked the method.

```js
var patrick = {name: "Patrick Star"};
patrick.sayHi = spongebob.sayHi;

patrick.sayHi(); // "Hi! I am Patrick Star"
```

## The Global Object

In browser's context, the global object is called `window`.
Everything declared in the global scope will automatically be a property of `window`.

```js
var myGlobal = "Global Variable";

// The statement above is interpreted as:
window.myGlobal = "Global Variable";

// Both will work:
alert(myGlobal); // "Global Variable"
alert(window.myGlobal); // "Global Variable"
```

A function in the global scope is considered as a method of the global object.

```js
// Both statements will achieve the same result:
function foo(){};
window.foo = function(){};
```

Any function **NOT** invoked by any object is considered to be invoked by the global object. Meaning, the value of `this` will be the global object, if a method is not invoked by any object.

```js
function foo(){
  console.log(this);
}

// Both statements will have the same output.
foo(); // window
window.foo(); // window
```
