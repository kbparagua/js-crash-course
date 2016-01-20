# 05: Object-Oriented Programming

## Constuctor

```js
function person(name){
  // this refers to the new object
  this.name = name;
}

var bruce = new person("bruce"); // {name: "bruce"}
```

It is a convention in the javascript community to capitalize the first letter of the constructor name.

```js
function Person(name){
  this.name = name;
}

var bruce = new Person("bruce");
```
