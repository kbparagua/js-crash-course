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
function container(x){
  function inside(){
    var y = 100;
    return x + y;
  }
  
  x = 100;
  
  // Execute inner function.
  inside();
}

//   _________________________________
//  |(container)env1:                 |
//  |                                 |
//  | x -> 100                        |
//  |                                 | 
//  |            ---> (container)env1 |
//  | inside ---|                     |
//  |            ---> function(){}    |
//  |                                 |
//  |   _________________________     |
//  |  |(inside)env2:            |    |
//  |  |                         |    |
//  |  | y -> 100                |    |
//  |  |_________________________|    |
//  |_________________________________|
//
container(1); // 200
// (inside)env2 will be destroyed.
// (container)env1 will be destroyed.
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

## Exercise

Re-write the function `initAdders` to achieve the correct behavior.

```js
var adders = [];

function initAdders(){
  for (var factor = 1; factor <= 5; factor++){
  	var addFactorTo = function(number){ return number + factor; }
		adders[factor] = addFactorTo;
	}
}

initAdders();

var addOneTo = adders[1];
console.log( addOneTo(3) ); // 9
```

## References
- http://stackoverflow.com/a/111111
