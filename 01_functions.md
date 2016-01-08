# 01: Functions

## JS Functions are First-class Citizens

*"First-Class Citizen is an entity that can be constructed at run-time, passed as a parameter, returned from a subroutine, or assigned into a variable"* [(wikipedia)](https://en.wikipedia.org/wiki/First-class_citizen)

- Assign to a variable.

  ```js
  function hello(){ alert("Hello"); }
  var sayHello = hello;
  ```
  
- Pass as a parameter.

  ```js
  function execute(myFunction){
    myFunction();
  }
  
  function hello(){ alert("Hello"); }
  execute(hello);
  ```

- Return by a function.

  ```js
  function getHelloFunction(){ 
    function hello(){ alert("Hello"); }
    return hello;
  }
  
  var sayHello = getHelloFunction();
  ```

- Construct at run-time.

  ```js
  var text = prompt("What to say?");
  var sayText = new Function("alert('" + text + "');");
  ```

## Function Expressions

Function expressions are just function declaration used as an expression.
```js
// Assigning the function hello to the variable myFunction.
var myFunction = function hello(){ alert("Hello"); };

alert(function hi(){ alert("Hi"); });
```

Function name can be ommited on function expressions.
```js
// Assigning an anonymous function to the variable myFunction.
var myFunction = function(){ alert("Hello"); };

alert(function(){ alert("Hi"); });
```

## Hoisting

Declaring a function anywhere is equivalent to declaring it at top.

Example:
```js
foo();

function foo(){
  alert("foo");
}
```

The code above will be interpreted as:
```js
function foo(){
  alert("foo");
}
foo();
```

### Function Expressions

Function expressions are not hoisted. They are treated like a regular variable declaration.
```js
foo(); // ReferenceError: foo is not defined
var foo = function(){ alert("foo"); }
```

The code above will be interpreted as:
```js
var foo;
foo(); // ReferenceError: foo is not defined
foo = function(){ alert("foo"); }
```

## Closures

A function declared inside another function has access to the local variables of the containing function.

```js
function sayName(){
  var name = "Pedro";
  
  function say(){ 
    // can access local variable of containing function `sayName`.
    alert(name);
  }
  
  say();
}

sayName(); // "Pedro"
```

The inner function is **NOT** copying the local variables of the containing function, it is just keeping references of them.
So any changes on the referenced local variables, even after the inner function declaration, will still be reflected.

```js
function whatsMyNumber(){
  var number = 1;
  
  function sayMyNumber(){ alert(number) };
  
  // Change the number AFTER the inner function delcaration
  number = 2;
  
  sayMyNumber();
}

whatsMyNumber(); // 2
```


## References
- http://stackoverflow.com/a/111111
