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

When a function is declared inside another function, the inner function will store a reference to the environemnt of the containing function.

```js
function container(x){
  function inside(){
    var y = 100;
    return x + y;
  }
}

//
// The following environment will be created when `container(1)` is executed. 
// Notice that `inside` will store references for both the inner function
// and the current environment.
//   _________________________________
//  |(container)env1:                 |
//  |                                 |
//  | x -> 1                          |
//  |                                 |
//  |            ---> (container)env1 |
//  | inside ---|                     |
//  |            ---> function(){}    |
//  |_________________________________|
//
container(1);
// (container)env1 will be destroyed.
```

Since the inner function has a reference to the current environment, it can access the arguments/variables available on that environment when it is executed.

```js
function container(x){
  function inside(){
    var y = 100;
    return x + y;
  }
  
  // Execute inner function.
  inside();
}

// Since `inside` has a reference to (container)env1, when it is executed, 
// it will create a new environment, (inside)env2, within (container)env1.
// It is an environment within another environment (a box inside another box).
//
// Because of that structure, (inside)env2 will have access to the arguments 
// and variables of (container)env1.
//   _________________________________
//  |(container)env1:                 |
//  |                                 |
//  | x -> 1                          |
//  |                                 |
//  |            ---> (container)env1 |
//  | inside ---|                     |
//  |            ---> function(){}    |
//  |                                 |
//  |   __________________________    |
//  |  |(inside)env2:             |   |
//  |  |                          |   |
//  |  | y -> 100                 |   |
//  |  |__________________________|   |
//  |_________________________________|
//
//
container(1); // 101
// (inside)env2 will be destroyed.
// (container)env1 will be destroyed.
```

`inside` here is an example of a closure. **Closure** is composed of a function and an environment, and it can be created by declaring a function inside another function.

## Arguments and Variables

The inner function **DOES NOT** copy the arguments and variables of the containing function. So any changes inside the containing function, even after the inner function declaration, will still be reflected when the inner function is executed.

```js
function foo(x){
  function bar(){
    var y = 100;
    return y + x;
  }
  
  // Change the value of `x`.
  x = 100;
  
  // Execute inner function.
  bar();
}

//   ___________________________
//  |(foo)env1:                 |
//  |                           |
//  | x -> 100                  |
//  |                           |
//  |         ---> (foo)env1    |
//  | bar ---|                  |
//  |         ---> function(){} |
//  |                           |
//  |  _______________________  |
//  | |(bar)env2:             | |
//  | |                       | |
//  | | y -> 100              | |
//  | |_______________________| |
//  |___________________________|
//
foo(1); // 200
// (bar)env2 will be destroyed.
// (foo)env1 will be destroyed.
```

## Returning A Closure

```js
//
// This is what we'll have on our global scope:
//
// makeAdder -> function(){}
//
function makeAdder(number){
  return function(x){
    return x + number;
  };
}


// This will be the environment when `var addFiveTo = makeAdder(5)` is executed.
//
// makeAdder -> function(){}
//
//               ---> (makeAdder)env1
// addFiveTo ---|                       
//               ---> function(){}
//  _______________________
// |(makeAdder)env1:       |
// |                       |
// | number -> 5           |
// |_______________________|
//
var addFiveTo = makeAdder(5);
// (makeAdder)env1 will NOT be destroyed because it is still referenced by the function `addFiveTo`.

//
// makeAdder -> function(){}      
//
//               ---> (makeAdder)env1   
// addFiveTo ---|                       
//               ---> function(){}      
//  _________________________________
// |(makeAdder)env1:                 |
// |                                 |
// | number -> 5                     |
// |  _____________________________  |
// | |(addFiveTo)env2:             | |
// | |                             | |
// | | x -> 10                     | |
// | |_____________________________| |
// |_________________________________|
//
addFiveTo(10); // 15
// (addFiveTo)env2 will be destroyed.
// (makeAdder)env1 will NOT be destroyed.
```

## References
- http://stackoverflow.com/a/111111
