# 05: Object-Oriented Programming

## Constructor

`new` is a special keyword used together with a function invocation to create an object.

```js
function regularFunction(){
  // do something here
}

var obj = new regularFunction(); // {}
```

If a function is invoked with `new`, it will be interpreted as if a new empty object invoked it, and will also return that new object.

```js
function zombie(){
  this.speed = 1.5;
}

var z = new zombie(); // {speed: 1.5}
```

The code above can be interpreted like:

```js
function zombie(){
  this.speed = 1.5;
  return this;
}

// invoke zombie in context of a new empty object
var z = zombie.call({}); // {speed: 1.5}
```

`zombie` is an example of a constructor, a function that is used to construct an object. To create a visual distinction between a regular function and a constructor, it is a common practice to capitalize the first letter of a constructor.

```js
function Zombie(){
  this.speed = 1.5;
}

var z = new Zombie();
```

## Prototype

Prototype is an object and a property of the constructor in which the created objects will inherit to.

```js
function Person(name){
  this.name = name;
}

Person.prototype = {
  x: "inerited property",
  talk: function(){ alert("I am " + this.name); }
};

var spongebob = new Person("spongebob"),
    patrick = new Person("patrick");
    
spongebob.talk(); // "I am spongebob"
patrick.talk(); // "I am patrick"

patrick.x; // "inherited property"
```

```js
//           ____________
//          |            |
//          | prototype  |
//          |____________|
//                |
//           _____|_____ 
//          /           \
//         /             \
//        |               |
//   _____|_____     _____|_____
//  |           |   |           |
//  | spongebob |   |  patrick  |
//  |___________|   |___________|
```
