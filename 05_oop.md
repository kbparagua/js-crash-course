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

Prototype is an object and a property of the constructor in which all constructed objects will inherit to. By default, it is an empty object.

```js
function Zombie(){
  this.speed = 1.5;
}

Zombie.prototype = {
  talk: function(){ alert("brainzzz..."); }
};

//      Zombie.prototype:
//   -- { talk: function(){ alert("brainzzz..."); } }
//  |                                             
//  |
//  |
//   ---> zpongebob: {speed: 1.5}
//  |   
//   ---> vatrick: {speed: 1.5}

var zpongebob = new Zombie(),
    vatrick = new Zombie();
    
zpongebob.talk(); // "brainzzz..."
```

It is a common practice to put common properties on the constructor's prototype.

```js
function Zombie(){}

Zombie.prototype = {
  speed: 1.5,
  talk: function(){ alert("brainzzz..."); }
};

//   -- Zombie.prototype:
//  |   { 
//  |     speed: 1.5,
//  |     talk: function(){ alert("brainzzz..."); }
//  |   }
//  |                                             
//  |
//  |
//   ---> zpongebob: {}
//  |   
//   ---> vatrick: {}

var zpongebob = new Zombie(),
    vatrick = new Zombie();
    
zpongebob.talk(); // "brainzzz..."

alert( vatrick.speed ); // 1.5
```

The constructed objects **DO NOT** copy the properties of the prototype, they will just search their constructor's prototype when an unknown key is accessed.

```js
// vatrick: Searching `talk` in my property list.
// vatrick: I don't have a property `talk`.
// vatrick: Searching `talk` in my constructor's prototype.
// vatrick: I found it.
// vatrick: Invoke `talk` method of my contructor's prototype.
vatrick.talk();
```

## Methods in Prototype

When a method inside the prototype is called by a constructed object, `this` will be equal to the constructed object **NOT** the prototype.

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype = {
  talk: function(){ alert("I am " + this.name + "... brainzzz..."); }
};

var zpongebob = new Zombie("zpongebob");
zpongebob.talk(); // "I am zpongebob... brainzzz..."
```

## Private Properties in Prototype

There is no way to create a private properties in javascript. All properties inside the prototype are all accessible through the constructed objects.

As a convention, programmers are using `_` underscore to prefix properties that are intended for private use.

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype = {
  imPublic: function(){
    alert("you can call me outside");
    
    // It is okay to access private property here.
    this._imPrivate();
  },
  
  _imPrivate: function(){ alert("do not call me outside"); }
};

var krabz = new Zombie("krabz");
krabz._imPrivate(); // "do not call me outside"
```

## Relationship

```js
function Zombie(){};
Zombie.prototype = {};

var zpongebob = new Zombie();

//          _________________________
//   ----> | Zombie => function(){}: |
//  |   -- | prototype               |
//  |  |   |_________________________|
//  |  |                                  ____________
//  |  |     ___________________         | zpongebob: |
//  |   --> | Zombie.prototype: | <----- | __proto__  |
//   ------ | constructor       |        |____________|
//          |___________________|

```
