# 06: Exercises

1. What's the output?
  
  ```js
  function wazeng(arg){
    arg.foo = function(){ alert("BAR"); };
    return arg;
  }
  
  var obj = {};
  obj["foo"] = function prettyFunction(){ alert("foo"); };
  
  var test = wazeng(obj);
  
  test.foo();
  obj.foo();
  ```
  
1. What's the output?

  ```js
  (function myApp(){
    this.name = "My Test Application";
    this.version = "1.0.0";
    this.env = "development";
  })();
  
  function Human(){};
  
  function Zombie(name){
    function init(){ 
      this.name = name;
      
      this.whenHumanDetected = function(){
        alert("brainzzz!!!!");
      };
    }
    
    init();
  }
  
  Zombie.prototype.encountered = function(object){
    if (object instanceof Human){
      alert(this.name + " encountered a human!");
      this.whenHumanDetected();
    }
  }
  
  var francis = new Human();
  var vatrick = new Zombie('vatrick');
  
  vatrick.encountered(francis);
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

1. What's the output?

  ```js
  function foo(){
  	var x = arguments.length * 2;
    return x;
  }
  
  var object = {x: 1};
  var result = foo.call(object, [0, 9, 8, 7, 6]);
  
  alert(result);
  ```
