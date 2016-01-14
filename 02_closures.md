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
//
foo(1); // 3
// (foo)env1 will be destroyed.

//  A new environment will be created during the execution of foo(5).
//   _____________
//  |(foo)env2:   |
//  |             |
//  | x -> 5      |
//  | y -> 2      |
//  |_____________|
//
foo(5); // 7
// (foo)env2 will be destroyed.
```

## Nested Environments

When a function is declared inside another function, the inner function will store a reference to the environment of the containing function.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
}

//
//  This environment will be created during execution of `outside(1)`.
//   ____________________________
//  |(outside)env1:              |
//  |                            |
//  | o -> 1                     |
//  |                            |
//  |          --> (outside)env1 |
//  | inside -|                  |
//  |          --> function(){}  |
//  |____________________________|
//
outside(1);
// (outside)env1 will be destroyed.
```

When the inner function is executed, it will create a bridge/connection with the outer function's environment. With that bridge, it can access any arguments or variables inside the connected environment.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
  
  inside();
}

// (inside)env2 can access argument and variables in (outside)env1, because of the bridge between them.
//   _________________              _____________________________
//  |(inside)env2:    |            |(outside)env1:               |
//  |                 |____________|                             |
//  | i -> 100         ____________  o -> 1                      |
//  |_________________|            |          --> (outside)env1  |
//                                 | inside -|                   |
//                                 |          --> function(){}   |
//                                 |_____________________________|
//
outside(1); // 101
// (inside)env2 will be destroyed.
// (outside)env1 will be destroyed.
```

`inside` here is an example of a closure. **Closure** is composed of a function and an environment, and it can be created by declaring a function inside another function.

## Arguments and Variables

The inner function **DOES NOT** copy the arguments and variables of the outer function. So any changes inside the outer, even after the inner function declaration, will still be reflected when the inner function is executed.

```js
function outside(o){
  function inside(){
    var i = 100;
    return o + i;
  }
  
  o = 100;
  inside();
}

//
//                              This is NOT a CLONE!
//   __________________          ____________________________
//  |(inside)env2:     |        |(outside)env1:              |
//  |                  |________|                            |
//  | i -> 100          ________  o -> 100                   |
//  |__________________|        |          --> (outside)env1 |
//                              | inside -|                  |
//                              |          --> function(){}  |
//                              |____________________________|
//
outside(1); // 200
// (inside)env2 will be destroyed.
// (outside)env1 will be destroyed.
```

## Returning A Closure

Normally, when an inner function is created, it will not be executed yet, rather it will be returned by the containing function. This is the usual use of closures.

In the example below, instead of executing the inner function instantly, we are just going to return a reference to the inner function.

```js
//
//  Global scope:
//
//  makeAdder -> function(){}
//
function makeAdder(number){
  function adder(x){
    return x + number;
  }
  
  return adder;
}
```

Or we can just return an anonymous function expression.

```js
//
//  Global scope:
//
//  makeAdder -> function(){}
//
function makeAdder(number){
  return function(x){
    return x + number;
  };
}
```

This is how we'll use the `makeAdder` function.

```js
//
//  Global scope:
//
//  makeAdder -> function(){}
//
//                ---> (makeAdder)env1
//  addFiveTo ---|                       
//                ---> function(){}
//   _______________________
//  |(makeAdder)env1:       |
//  |                       |
//  | number -> 5           |
//  |_______________________|
//
var addFiveTo = makeAdder(5);
// (makeAdder)env1 will NOT be destroyed because it is still referenced by the function `addFiveTo`.

//
//  Global scope:
//  makeAdder -> function(){}      
//
//                ---> (makeAdder)env1   
//  addFiveTo ---|                       
//                ---> function(){}      
//   _________________________________
//  |(makeAdder)env1:                 |
//  |                                 |
//  | number -> 5                     |
//  |   ___________________________   |
//  |  |(addFiveTo)env2:           |  |
//  |  |                           |  |
//  |  | x -> 10                   |  |
//  |  |___________________________|  |
//  |_________________________________|
//
addFiveTo(10); // 15
// (addFiveTo)env2 will be destroyed.
// (makeAdder)env1 will NOT be destroyed!
```

Notice that the environment of the containing function is never destroyed. This is because it is still used by the closure `addFiveTo`.

## References
- http://stackoverflow.com/a/111111
