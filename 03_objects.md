# 03: Objects

## Creating Basic Object

Object is a collection of properties, wherein each property is composed of a key-value pair.

```js
var bruce = {"firstname": "Bruce", "lastname": "Lee"};
```

### Keys

Keys are automatically converted to strings, so we can ommit the quotes around the keys.

```js
var bruce = {firstname: "Bruce", lastname: "Lee"};
```

If quotes are ommited, a key should be a valid identifier name.

```js
// This will not work.
var obj = {4x4: "four by four"}; // Syntax Error

// Must enclose with quotes.
var obj = {"4x4": "four by four"};
```

If a non-string key is provided, such as a number or a boolean, it will be typecasted to string.

```js
var object = {false: "false value", 100: "one hundred"};

// The object above is equivalent to this object:
{"false": "false value", "100": "one hundred"};
```

### Values

Property values can be any expressions.

```js
var bruce = {
  firstname: "Bruce",
  lastname: "Lee",
  age: 75,
  alive: false,
  family: {wife: "Linda", son: "Brandon", daughter: "Shannon"},
  kick: function(){ alert("watta!"); }
};
```

As you can see, we can even use another object as a property value. Basically, anything that can be assigned to a variable can also be a property value.

## Accessing Properties

We use the dot operator (`.`) to access properties. 

```js
alert(bruce.lastname); // "Lee"
```

Use the same operator to manipulate or add properties.

```js
// Change property value.
bruce.age = 76;

// Add new property.
bruce.lastMovie = "Game of Death";
```

### Irregular Keys

Dot operator can only access properties with keys that are valid identifier name.

```js
var obj = {"4x4": "Four by Four"};
alert(obj.4x4) // Syntax Error
```

Use Bracket notation (`[]`) to access irregular keys.

```js
var obj = {"4x4": "Four by Four"};
alert(obj["4x4"]); // "Four by Four"
```

