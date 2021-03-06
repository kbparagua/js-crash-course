# 04: Objects

An Object is a collection of properties, wherein each property is composed of a key-value pair.

```js
var bruce = {"firstname": "Bruce", "lastname": "Lee"};
```

## Keys

Property key, also referred as property name, is automatically converted to string. So quotes can be ommited when writing property names.

```js
var bruce = {firstname: "Bruce", lastname: "Lee"};
```

Using this format, property name should be a valid indentifier name.

```js
// Will not work
var obj = {4x4: "four by four"}; // Syntax Error

// Must enclose with quotes to work.
var obj = {"4x4": "four by four"};
```

If a non-string key is provided, such as a number or a boolean, it will be typecasted to string.

```js
// typecasted to:
//  {"false": "false value", "100": "one hundred"}
var object = {false: "false value", 100: "one hundred"};
```

## Values

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

Basically, anything that can be assigned to a variable can also be a property value.

## Accessing Properties

Dot operator (`.`) is used to access properties. 

```js
var bruce = {firstname: "Bruce", lastname: "Lee"};
alert(bruce.lastname); // "Lee"
```

Use the same operator to manipulate or add properties.

```js
var bruce = {firstname: "Bruce", lastname: "Lee"};

// Change property value.
bruce.lastname = "Wayne";

// Add new property.
bruce.nickname = "Batman";
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

### Bracket Notation

Generally, bracket notation (`[]`) is used to access keys using a variable or any expression.

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

// typcasted:
//   alert(obj["100"]);
alert(obj[50 + 50]); // "One Hundred"
```

## Object as Argument

When an object is created, javascript returns a reference instead of the object itself. Meaning, when dealing with an object, we are really talking to a reference rather than the object itself.

```js
// [reference:01] -> {myKey: "value"}
// myObject -> [reference:01]

var myObject = {myKey: "value"};
```

When an object is passed as an argument, a reference of it is actually passed. So, manipulating that reference will reflect the change to the real object.

```js
function changeMe(object){
  // object --> [reference:01] => {x: "unchanged"}
  object.x = "changed";
  // object --> [reference:01] => {x: "changed"}
}

var obj = {x: "unchanged"};
// obj --> [reference:01] => {x: "unchanged"}

changeMe(obj);
// obj --> [reference:01] => {x: "changed"}

alert(obj.x); // "changed"
```

But assigning the argument to a different object/reference will not affect the old object/reference.

```js
function changeMe(object){
  // object --> [reference:01] => {x: "unchanged"}
  object = {x: "changed"};
  // object --> [reference:02] => {x: "changed"}
}

var obj = {x: "unchanged"};
// obj --> [reference:01] => {x: "unchanged"}

changeMe(obj);

alert(obj.x); // "unchanged"
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

The value of `this` is evaluated at run-time, so it will always be equal to the object that invoked the method.

```js
var spongebob = {
  name: "Spongebob Squarepants",
  sayHi: function(){
    alert("Hi! I am " + this.name);
  };
};

var patrick = {name: "Patrick Star"};
patrick.sayHi = spongebob.sayHi;

patrick.sayHi(); // "Hi! I am Patrick Star"
```

## The Global Object

In browser's context, the global object is called `window`.
Everything declared in the global scope will automatically be a property of `window`.

```js
// Both statements will achieve the same result.
var myGlobal = "Global Variable";
window.myGlobal = "Global Variable";

// Both will work:
alert(myGlobal); // "Global Variable"
alert(window.myGlobal); // "Global Variable"
```

A function in the global scope is also considered a method of the global object.

```js
// Both statements will achieve the same result.
function foo(){};
window.foo = function(){};
```

Any function **NOT** invoked by any object is considered to be invoked by the global object. Meaning, the value of `this` will be the global object if a function is not invoked by any object.

```js
function foo(){
  console.log(this);
}

// Both statements will have the same output.
foo(); // window
window.foo(); // window
```

## Global `this`

In the global scope, `this` refers to the global object.

```js
var theGlobalObject = this;

function foo(){
  alert(theGlobalObject === this);
}

alert(theGlobalObject === window); // true
foo(); // true
```

## Changing This

A way to change the value of `this` on runtime is by using the function `call` or `apply`.

```js
function talk(){
  alert(this.name);
}

var batman = {name: "Batman"},
    robin = {name: "Robin"};

// Execute talk in the context of batman.
// Inside talk, `this` will be equal to batman.
talk.call(batman); // "Batman"

// Execute talk in the context of robin.
// Inside talk, `this` will be equal to robin.
talk.apply(robin); // "Robin"
```

Passing arguments to the function is possible when invoking functions using `call` and `apply`.
When using `call` the arguments are separated by comma.

```js
function talk(prefix, suffix){
  alert(prefix + this.name + suffix);
}

var batman = {name: "Batman"};

talk.call(batman, "I am ", "!"); // "I am Batman!"
```

When using `apply` the arguments must be inside an array.

```js
var robin = {name: "Robin"};

talk.call(robin, ["Hello! I am ", "!!!"]); // "Hello! I am Robin!!!"
```

Basically, `call` and `apply` are just the same, they just differ in the way they accept arguments.

- **A**pply - **A**rray
- **C**all - **C**omma
