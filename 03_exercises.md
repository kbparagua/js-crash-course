# 03: Exercises

## Exercise

1. What's the output?

  ```js
  var x;
  alert(x + 5);
  ```
  
1. What's the output?

  ```js
  var string = "";
  
  if (string && true){
    alert("YES");
  }
  else {
    alert("NO");
  }
  ```

1. What's the output?

  ```js
  var y;
  
  if (y == null){
    alert("YEAH");
  }
  else {
    alert("NAH");
  }
  ```

1. What's the output?

  ```js
  alert(x);
  
  foo();
  alert(x);
  
  var x = 1;
  
  function foo(){ x = 2; };
  ```

1. What's the output?

  ```js
  var x = 1;
  
  function bar(){
    var x = 9;
    x = x + 1;
  }
  
  bar();
  alert(x);
  ```
  
1. What's the output?

  ```js
  var bar = foo();
  bar();
  
  var foo = function(){ return "foo"; };
  ```

1. What's the output?

  ```js
  var y = "Hello";
  
  (function(x){ 
    var string = "World",
        msg = x + " " + string;
        
    alert(msg); 
  })(y);
  
  alert(string);
  ```
  
1. Re-write the function `initAdders` to achieve the correct behavior.

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
  console.log( addOneTo(3) ); // 9 - should be 4
  ```

