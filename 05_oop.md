# 05: Object-Oriented Programming

## Constructor

A constructor is just a regular function that is used to create an object.

```js
function person(name){
  // this refers to the new object
  this.name = name;
}

var bruce = new person("bruce"); // {name: "bruce"}
```

It is a convention in the javascript community to capitalize the first letter of the constructor name.

```js
function Person(name){
  this.name = name;
}

var bruce = new Person("bruce");
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
