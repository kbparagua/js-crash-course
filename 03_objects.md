# 03: Objects

## Creating Basic Object

Object is a collection of properties, wherein each property is composed of a key-value pair.

```js
var myObject = {"key1": "value01", "key2": "value02"};
```

Keys are automatically converted to strings, so you ommit the quotes around the keys.

```js
var myObject = {key1: "value01", key2: "value02"};
```

Property values can be any expressions.

```js
var myObject = {
  key1: "hello",
  key2: 1,
  key3: false,
  key4: function(){ alert("hi"); },
  key5: {key1: 1, key2: 2}
};
```

As you can see, we can even use another object as a property value. Basically, anything can be assigned to a variable can be also be a property value.

## Accessing Properties

We use the dot operator (`.`) to access properties. 

```js
var myObject = {key1: "value01", key2: "value02"};

// Get the value of the property with a string key equal to "key1".
alert(myObject.key1); // "value01"
```

Use the same operator to manipulate or add properties.

```js
var myObject = {key1: "value01", key2: "value02"};

// Change key1's value.
myObject.key1 = "new value";

// Add a new property.
myObject.key3 = "new property";
```


## Non-String Keys

Non-string keys cannot be used directly when creating an object literal. 

```js
// This will NOT create a property with the number 100 as a key.
//
// This will be the object that will be created: {"100": "my value"}
var myObject = {100: "my value"};
```

Bracket notation must be used instead of the dot operator to add properties with non-string keys.

```js
// Create an empty object first.
var myObject = {};

// Create a property with the number 100 as the key.
myObject[100] = "my value";
```

Dot operator will always look for string keys. Make sure to use the bracket notation when dealing with non-string keys.

```js
var myObject = {};
myObject[100] = "Hello World";

alert(myObject.100); // Syntax Error
```

## Accessing Properties

1. Dot operator - `.`
  
  - Access property ONLY with string keys.

  ```js
  var myObject = {key1: "value01", key2: "value02"};
  
  // Access the property with the a string key equal to "key1"
  alert(myObject.key1);
  ```

2. Bracket notation - `[]`

  - Access property with the specified key.
  
  ```js
  var myObject = {key1: "value01", };
  ```
