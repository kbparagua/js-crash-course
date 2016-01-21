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

Prototype is an object and a property of the constructor in which all constructed objects will inherit to.

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

