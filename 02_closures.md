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

## References
- http://stackoverflow.com/a/111111
