# 02: Closures

## Function Environment

For the sake of simplicity, let's use the following definition.

An environment is a temporary place (*let's just say a box*) where arguments and local variables are stored when a function is executed. It is created at the start of the execution and *normally destroyed after execution is finished*.

```js
function foo(x){
  var y = 2;
  return x + y;
}

//  A new environment will be created during the execution of foo(1).
//   _____________
//  |(foo)env1:   |
//  |             |
//  | x -> 1      |
//  | y -> 2      | 
//  |_____________|

foo(1); // 3

// (foo)env1 will be destroyed.

//  A new environment will be created during the execution of foo(5).
//   _____________
//  |(foo)env2:   |
//  |             |
//  | x -> 5      |
//  | y -> 2      |
//  |_____________|

foo(5); // 7

// (foo)env2 will be destroyed.
```

## Linked Environments

When a function is declared inside another function, the inner function will store a reference of the containing function's environment.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
}

//  `inside` will store a reference of both the inner function and the current environment.
//   ____________________________
//  |(outside)env1:              |
//  |                            |
//  | o -> 1                     |
//  |                            |
//  |          --> (outside)env1 |
//  | inside -|                  |
//  |          --> function(){}  |
//  |____________________________|

outside(1);

// (outside)env1 will be destroyed.
```

When the inner function is executed, it will create a bridge between its own environment and its stored environment. Because of that bridge, the inner fuction can access any arguments and variables of the containing function.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
  
  inside();
}

//  (inside)env2 can access argument and variables of (outside)env1, because of the bridge between them.
//   ____________________________
//  |(outside)env1:              |
//  |                            |
//  | o -> 1                     |
//  |          --> (outside)env1 |
//  | inside -|                  |
//  |          --> function(){}  |
//  |_____    ___________________|
//        |  |
//        |  |
//   _____|  |________
//  |(inside)env2:    |      
//  |                 |
//  | i -> 100        | 
//  |_________________|

outside(1); // 101

// (inside)env2 will be destroyed.
// (outside)env1 will be destroyed.
```

`inside` here is an example of a closure. 

**Closure** is a combination of a function and a reference to an environment, and it can be created by declaring a function inside another function.

## Arguments and Variables

The inner function **DOES NOT** copy the arguments and variables of the outer function. So any changes that will happen inside the containing function environment will be available to the inner function environment.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
  
  // Change the value of `o` after inner function declaration.
  o = 100;
  
  inside();
}

//   ____________________________
//  |(outside)env1:              |
//  |                            |
//  | o -> 100                   |
//  |          --> (outside)env1 |
//  | inside -|                  |
//  |          --> function(){}  |
//  |_____    ___________________|
//        |  |
//        |  |
//   _____|  |_______
//  |(inside)env2:   |
//  |                |
//  | i -> 100       |
//  |________________|

outside(1); // 200

// (inside)env2 will be destroyed.
// (outside)env1 will be destroyed.
```

## Returning a Closure

Normally, when an inner function is created, it will not be executed immediately. Most of the time, the closure will be returned by the containing function, or it will be passed to another function.

In the example below, instead of executing the inner function immediately, a reference of it is returned by the containing function.

```js

//  Global scope:
//
//  makeAdder -> function(){}

function makeAdder(number){
  function adder(x){
    return x + number;
  }
  
  return adder;
}
```

It is also possible to just return an anonymous function expression.

```js

//  Global scope:
//
//  makeAdder -> function(){}

function makeAdder(number){
  return function(x){
    return x + number;
  };
}
```

Notice that the `makeAdder` function acts like a function builder. Also, it is not building just a regular function, but it is creating and returning a closure.

```js

//  Global scope:
//
//  makeAdder -> function(){}
//
//              --> (makeAdder)env1
//  addFiveTo -|                       
//              --> function(){}

//   _____________________
//  |(makeAdder)env1:     |
//  |                     |
//  | number -> 5         |
//  |_____________________|

var addFiveTo = makeAdder(5);

// (makeAdder)env1 will NOT be destroyed because it is still referenced by the closure `addFiveTo`.
```

Normally, after a function's execution, its environment is destroyed. But in this instance, since the closure was returned and not executed inside `makeAdder`, it's like telling the system that the environment `(makeAdder)env1` is still needed by the script.

```js
//  Global scope:
//
//  makeAdder -> function(){}      
//
//                ---> (makeAdder)env1   
//  addFiveTo ---|                       
//                ---> function(){}   

//   ___________________
//  |(makeAdder)env1:   |
//  |                   |
//  | number -> 5       |
//  |___    ____________|
//      |  |
//      |  |
//   ___|  |___________
//  |(addFiveTo)env2:  |
//  |                  |
//  | x -> 10          |
//  |__________________|

addFiveTo(10); // 15

// (addFiveTo)env2 will be destroyed.
// (makeAdder)env1 will NOT be destroyed!
```

## Closure as an Argument

Closure will always reference the environment where it is created, and not where it is executed.

```js
function create(){
  var x = "Hello";
  
  // Create closure using anonymous function expression.
  execute(function(){
    var y = "World";
    return x + " " +y;
  });
}

function execute(myFunction){
  var x = "Hola";
  
  // Execute closure.
  alert(myFunction());
}

//   ________________
//  |(create)env1:   |
//  |                |
//  | x -> "Hello"   |
//  |___    _________|
//      |  |
//      |  |
//   ___|  |___________
//  |(myFunction)env2: |
//  |                  |
//  | y -> "World"     |
//  |__________________|

create(); // "Hello World"
```

## References
- http://stackoverflow.com/a/111111
