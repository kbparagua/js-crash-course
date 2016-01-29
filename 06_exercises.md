# 06: Exercises

1. What's the output?

```js
function Zombie(name){
  this.name = name;
}

Zombie.prototype.talk = function(){
  return "brainzzz...";
};

var z = new Zombie('z');

alert(z.contsructor);
alert(z.prototype);
```
