# 01: Functions

## JS Functions are First-class Citizens.

*"First-Class Citizen is an entity that can be constructed at run-time, passed as a parameter, returned from a subroutine, or assigned into a variable"* [(wikipedia)](https://en.wikipedia.org/wiki/First-class_citizen)

- Construct at run-time.

  ```js
  var text = prompt("What to say?");
  var sayText = new Function("alert('" + text + "');");
  ```

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
