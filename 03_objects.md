# 03: Objects

# Creating Basic Object

Object is a collection of properties, wherein each property is composed of a key-value pair.

```js
var myObject = {"key1": "value01", "key2": "value02"};
```

Keys are automatically converted to strings, so you can just create keys like this:

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

As you can see, we can even use another object as a property value.


## Accessing Properties

```js
var myObject = {key1: "value01", key2: "value02"};
```
