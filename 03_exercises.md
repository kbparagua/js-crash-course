# 03: Exercises

## Exercise

1. What's the output?

  ```js
  var str = "";
  
  if (str && true){
    if (0)
      alert("YES");
    else
      alert("I don't know");
  }
  else {
    alert("NO");
  }
  ```
  
1. What's the output?

  ```js
  var magicNumber;
  alert(magicNumber + 42);
  ```

1. What's the output?

  ```js
  function speak(x){
    if (x === null)
      alert("What?");
    else
      alert("Yes!");
  }
  
  speak();
  ```
  
1. What's the output?

  ```js
  var number = 1;
  
  if (number > 0){
    var str = "positive";
  }
  else if (number == 0){
    var str = "zero";
  }
  else {
    var str = "negative";
  }
  
  alert(str);
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
  function foo(x){
    function bar(){ x = 999; }
    return bar;
  }
  
  
  var num = 1000,
      foobar = foo(num);
  
  foobar();
  
  alert(num);
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
  
1. What's the output?

  ```js
  function foo(fx){
    var animal = "Dog";
    return fx();
  }
  
  function baz(){
    var animal = "Pig";
    
    return function(){
      if (animal == "Pig")
        return "Oink";
      else
        return "Woof"
    };
  }
  
  var sound = baz();
  alert(foo(sound));
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
  alert( addOneTo(3) ); // 9 - should be 4
  ```

