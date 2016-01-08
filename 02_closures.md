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

An environment is a place where arguments and local variables are stored for each function. Whenever a function is executed a new environment is created and then destroyed after its execution.

```js
function foo(x){
  var y = 2;
  return x + y;
}

//  A new environment will be created during the execution of foo(1).
//   ________
//  |        |
//  | x -> 1 |
//  | y -> 2 |
//  |________|
foo(1); // 3
// environment is destroyed after execution.

//  A new environment will be created during the execution of foo(5).
//   ________
//  |        |
//  | x -> 5 |
//  | y -> 2 |
//  |________|
foo(5); // 7
// environment is destroyed after execution.
```



## Closure

Closure is a pair of function and an environment.

## References
- http://stackoverflow.com/a/111111
