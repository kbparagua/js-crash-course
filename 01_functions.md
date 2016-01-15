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

## Scopes

Both function declaration and function expression create local function.

```js
function foo(){
  function privateA(){ alert("Private A"); }
  var privateB = function(){ alert("Private B"); };
}

privateA(); // ReferenceError: privateA is not defined.
privateB(); // ReferenceError: privateA is not defined.

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

## Immediately-Invoked Function Expression (IIFE)

Since the only way to limit the scope of variables is by enclosing them in a function, a function that is NOT intended for re-use is often created just a way to hide variables.

```js
var init = function(){
  var name = "Bruce Lee";
  var greeting = "Welcome";
  
  var message = greeting + ' ' + name + '!';
  alert(message);
};

init(); // Welcome Bruce Lee!

// name, greeting, and message is unavailable at this part of the code.
alert(name); // ReferenceError: name is not defined.
``` 

Since the `init` function is not intended to be re-used again, sometimes it is better to just execute it immediately after it is created.

```js
// Create and execute function.
(function init(){ 
  var name = "Bruce Lee";
  var greeting = "Welcome";
  
  var message = greeting + ' ' + name + '!';
  alert(message);
})(); // Welcome Bruce Lee!

// name, greeting, and message is unavailable at this part of the code.
alert(name); // ReferenceError: name is not defined.
```

Ideally, the function name can be removed since it will not be called again.

```js
// Create and execute function.
(function(){ 
  var name = "Bruce Lee";
  var greeting = "Welcome";
  
  var message = greeting + ' ' + name + '!';
  alert(message);
})(); // Welcome Bruce Lee!

// name, greeting, and message is unavailable at this part of the code.
alert(name); // ReferenceError: name is not defined.
```

## Arguments Object

`arguments`
  - an Array-like object corresponding to the arguments passed to a function.

If a function is declared with no arguments, arguments can still be passed to it.

```js
function printItems(){
  for (var i = 0, n = arguments.length; i < n; i++){
    var item = arguments[i];
    alert(item);
  }
}

printItems("pig", "cat", "dog"); // "pig" "cat" "dog"
```

If a function is declared with less arguments, more arguments can still be passed to it.

```js
function printItems(prefix){
  for (var i = 1, n = arguments.length; i < n; i++){
    var item = arguments[i];
    alert(prefix + item);
  }
}

printItems("my ", "pig", "cat", "dog"); // "my pig" "my cat" "my dog"
```

## Manipulating Arguments

Arguments are passed by value.

```js
function cook(arg){
  arg = "fried chicken";
}

var chicken = "chicken";

cook(chicken);

// Value of chicken will not change.
alert(chicken); // "chicken"
```
