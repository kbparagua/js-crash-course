# 06: Exercises

1. What's the output?

```js
function Human(name){
	this.name = name;
}

Human.prototype.talk = function(){
	alert("I'm " + this.name);
}; 

var john = new Human("john"),
		jane = new Human("jane");
    
 john.__proto__.talk();
```

1. What's the output?

```js
function Human(name){
	this.name = name;
}

Human.prototype.talk = function(){
	alert("I'm " + this.name);
}; 

var john = new Human("john"),
		jane = new Human("jane");
    
 var talk = john.__proto__.constructor.prototype.talk;
 talk.call(jane);
```

1. What's the output?

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype.talk = function(){
  return "brainzzz...";
};

var z = new Zombie('z');

alert(z.contsructor);
alert(z.prototype);
```

1. What's the output?

```js
function Shape(){}
function Square(){}
function Circle(){}

Circle.prototype = Shape.prototype;
Circle.prototype.sides = 0;

Square.prototype = Shape.prototype;
Square.prototype.sides = 4;

var circle = new Circle();
var x = new circle.constructor();

alert(x.constructor);
alert(x.sides);
```
