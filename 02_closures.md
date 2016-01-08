# 02: Closures


## Nested Function Declarations

A function declared inside another function has a reference of the local variables of the containing function.

```js
function sayName(){
  var name = "Pedro";
  
  function say(){ 
    // has reference of the local variable of containing function.
    alert(name);
  }
  
  say();
}

sayName(); // "Pedro"
```

The inner function also has references of the passed arguments to the containing function.

```js
function sayText(text){
  function say(){
    // has reference of the passed argument to the containing function.
    alert(text);
  }
  
  say();
}

sayText("Juan"); // "Juan"
```

The inner function is **NOT** copying the local variables of the containing function, it is just keeping references of them.
So any changes on the referenced local variables, even after the inner function declaration, will still be reflected.

```js
function sayNumber(){
  var number = 1;
  
  function alertNumber(){ alert(number); };
  
  // Change the number AFTER the inner function delcaration
  number = 2;
  
  alertNumber();
}

sayNumber(); // 2
```

## Function Environment

This is just a simplification.

An environment is a temporary place (*let's just say a box*) where arguments and local variables are stored when a function is executed. It is created at the start of the execution and normally destroyed after execution is finished.

```js
function foo(x){
  var y = 2;
  return x + y;
}

//  A new environment will be created during the execution of foo(1).

//   ________
//  |env1:   |
//  |        |
//  | x -> 1 |
//  | y -> 2 |
//  |________|
//
foo(1); // 3
// env1 is destroyed after execution.

//  A new environment will be created during the execution of foo(5).
//   ________
//  |env2:   |
//  |        |
//  | x -> 5 |
//  | y -> 2 |
//  |________|
//
foo(5); // 7
// env2 is destroyed after execution.
```

## Nested Environments

When a function is declared inside another function, the inner function will store a reference to the environemnt of the containing function.

```js
function foo(x){
  function bar(){
    var y = 100;
    return y + x;
  }
}

foo(1);
//
// During execution of `foo(1)`, `bar` will store references for both the inner function
// and the current environment.
//   ___________________________
//  |(foo)env1:                 |
//  |                           |
//  | x -> 1                    |
//  |                           |
//  |         ---> (foo)env1    |
//  | bar ---|                  |
//  |         ---> function(){} |
//  |___________________________|
//
```

Since the inner function has a reference to the current environment, it can access the variables available on that environment when it is executed.

```js
function foo(x){
  function bar(){
    var y = 100;
    return y + x;
  }
  
  // Execute inner function.
  bar();
}
// `bar()` will be executed inside the (foo)env1. Meaning, inside `bar` we can access the variable `x`.
//   ___________________________
//  |(foo)env1:                 |
//  |                           |
//  | x -> 1                    |
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
//
foo(1); // 101
```


## Closure

Closure is a pair of function and an environment.

## References
- http://stackoverflow.com/a/111111
