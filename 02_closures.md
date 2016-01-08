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

An environment is a place where arguments and local variables are stored for each function. Whenever a function is executed a new environment is created and normally destroyed after its execution.

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

Here's an illustration of a nested environment. Since the inner environment is enclosed inside the parent environment, it can access all variables in it.

```js
function foo(x){
  function bar(){
    var y = 100;
    return y + x;
  }
  
  bar();
}

// 2 new environments will be created. One for `foo` and since we are also
// executing `bar` inside, an environment will also be created for it.
//   ___________________
//  |(foo)env1:         |
//  |                   |
//  | x -> 1            |
//  | bar -> function() |
//  |  _______________  |
//  | |(bar)env2:     | |
//  | |               | |
//  | | y -> 100      | |
//  | |_______________| |
//  |___________________|
//
foo(1);
// env2 will be destroyed first, since bar() will end its execution first.
// lastly, env1 will be destroyed.
```


## Closure

Closure is a pair of function and an environment.

## References
- http://stackoverflow.com/a/111111
