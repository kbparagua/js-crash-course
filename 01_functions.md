# 01: Functions

## JS Functions are First-class Citizens

*"First-Class Citizen is an entity that can be constructed at run-time, passed as a parameter, returned from a subroutine, or assigned into a variable"* [(wikipedia)](https://en.wikipedia.org/wiki/First-class_citizen)

- Assign to a variable.

  ```js
  var sayHello = function(){ alert("Hello"); };
  ```
  
- Pass as a parameter.

  ```js
  function performThis(myFunction){
    myFunction();
  }
  
  performThis(function(){ alert("Hello"); });
  ```

- Return by a function.

  ```js
  function createSayHello(){
    return function(){ alert("Hello"); }
  }
  
  var sayHello = createSayHello();
  sayHello();
  ```

- Construct at run-time.

  ```js
  var text = prompt("What to say?");
  var sayText = new Function("alert('" + text + "');");
  ```

## Hoisting

Declaring a function anywhere is equivalent to declaring it at top.

Example:
```js
foo();
// other statements here
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
// other statements here
```
