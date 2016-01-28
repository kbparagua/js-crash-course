# 05: Object-Oriented Programming

## Inheritance

An object can inherit from another object by assigning the other object to its `__proto__` property.

```js
var human = {
  talk: function(){ alert("I am a human."); }
};

var goku = {__proto__: human};

goku.talk(); // "I am a human."
```

The parent, `human`, is an example of a **prototype**. Basically, a prototype is an object that acts as a parent of other object/s. 

### Default Prototype

By default, an object has an empty object (w/ a `null` prototype) as its prototype.

```js
var goku = {name: "Goku"};

console.log(goku.__proto__); // {}
console.log(goku.__proto__.__proto__); // null
```

### Prototype Chain

When a property is accessed in an object, it will search its own properties and will also include the properties of its whole prototype chain.

```js
var creature = {alive: true};
var human = { talk: function(){ alert("hello"); } };
var goku = {name: 'Goku');

human.__proto__ = creature;
goku.__proto__ = human;

// Prototype Chain:
//  goku.__proto__ --> human
//  human.__proto__ --> creature
//  creature.__proto__ --> {}
//  {}.__proto__ --> null

alert(goku.alive); // true
goku.talk(); // "hello"
alert(goku.name); // "Goku"
```

The child object will **NOT** copy the properties of its ancestors.

### Object.create

`Object.create` can also be used to create an object with the specified prototype.

```js
var parent = {inherited: "Inherited Property"};

// Create an object with parent as the prototype
var child = Object.create(parent);
child.own = "Own Property";

alert(child.inherited); // "Inherited Property"
alert(child.__proto__); // {inherited: "Inherited Property"}
```

### Prototype's methods

When a method in the prototype is executed by a child object, `this` will refer to the child object and not to the prototype.

```js
var human = {
  name: "anonymous",
  talk: function(){ alert("I am " + this.name); }
};

var goku = Object.create(human);
goku.name = "goku";

goku.talk(); // "I am goku"
```

## Constructor

A Constructor is just a regular function that is used together with the keyword `new` to construct a new object.

```js
function myFunction(){
  // Do something here
}

var obj = new myFunction; // {}
```

When an constructor is used together with `new`, it will be interpreted as if a new empty object invoked it, and will also return that new object.

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

Most of the time, constructor is not in verb form, rather they are in noun form which represents the type or class of object that they are constructing. Also, as a convention and to make them appear different from a regular function, they are written with a capitalized first letter.

```js
function Zombie(name){
  this.name = name;
}

var zpongebob = new Zombie("zpongebob");
```

## Constructor's Prototype

It is also possible to assign a prototype to all constructed objects by using the constructor's `prototype` property.

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype = {
  talk: function(){
    alert("brainzzz...");
  }
};

var zpongebob = new Zombie("zpongebob");

zpongebob.talk(); // "brainzzz..."
alert(zpongebob.__proto__ === Zombie.prototype); // true
```

By default, constructor's prototype is an empty object.

```js
function Zombie(name){
  this.name = name;
}

var vatrick = new Zombie("vatrick");
alert(vatrick.__proto__ === Zombie.prototype); // true
alert(Zombie.prototype); // {}
```

## Private Properties

Unlike other programming languages, there is no way to create private properties in javascript. As a workaround and also as a convention, programmers are using `_` underscore to prefix properties that are intended for private use.

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype.imPublic = function(){
  alert("you can call me outside");
    
  // It is okay to access private property here.
  this._imPrivate();
};
  
Zombie.prototype._imPrivate = function(){ 
  alert("do not call me outside");
};

var krabz = new Zombie("krabz");
krabz._imPrivate(); // "do not call me outside"
```

## Prototype-Constructor Relationship

A constructor's prototype will have a `constructor` property. Both the constructor and the prototype has a reference to each other.

```js
//   _____________
//  |Zombie:      | <------------------------------
//  |             |       ____________________     |
//  |  prototype  | ---> |Zombie.prototype:   |    |
//  |_____________|      |                    |    |
//                       |  constructor       | ---
//                       |____________________|

function Zombie(){};

Zombie.prototype = {
  talk: function(){
    alert("brainzzz...");
  }
};

console.log( Zombie.prototype.constructor ); // function Zombie(){}
console.log( Zombie.prototype ); // {talk: function(){}, constructor: Zombie}
```

## Class Inheritance

```js
function Human(name){
  this.name = name;
}

Human.prototype.sayName = function(){
  alert("I'm " + this.name);
};

function Programmer(name, language){
  Human.call(this, name);
  this.language = language;
}

Programmer.prototype = Object.create(Human.prototype);

Programmer.prototype.work = function(){
  alert("Code in " + this.language);
};

var francis = new Programmer("francis", "ruby");

francis.sayName(); // "I'm francis"
francis.work(); // "Code in ruby"

// francis.__proto__ --> Programmer.prototype --> Human.prototype --> {}.prototype --> null
```

## Checking Object Class

Use `instanceof` operator to check if an object is an instance of a specific class or created by a specific constructor.

```js
function Zombie(name){ this.name = name; }

var zpongebob = new Zombie('zpongebob');

alert(zpongebob instanceof Zombie); // true
```

