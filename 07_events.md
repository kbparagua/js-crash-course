# 07: Events

## Listeners

```js
var el = document.getElementById('test');

function listener(event){
  // do something
}

el.addEventListener('click', listener, false);
```

## Propagation Cycle

1. During the capture phase, the farthest ancestor will be visited first, down to the target element.
2. During the bubble phase, the target element will be visited first, up to the farthest ancestor.

```js
//   ______________________
//  |element_1             |  ||            /\
//  |   ________________   |  ||            ||
//  |  |element_2       |  |  || Capture    || 
//  |  |   __________   |  |  ||            || Bubble
//  |  |  |element_3 |  |  |  ||            ||
//  |  |  |__________|  |  |  \/            ||
//  |  |________________|  |              
//  |______________________|              
// 
```

**Capture Phase** order:
  1. `element_1`
  1. `element_2`
  1. `element_3`

**Bubble Phase** order:
  1. `element_3`
  1. `element_2`
  1. `element_1`


## Stop Propagation

```html
<div id="outer">
  <div id="inner">
  </div>
</div>
```

```js
var outer = document.getElementById('outer'),
    inner = document.getElementById('inner');
  
outer.addListener('click', function(e){
  alert('outer clicked');
}, false);

inner.addListener('click', function(e){
  alert('inner clicked');
  e.stopPropagation();
}, false);
```

`outer`'s click listener will not execute when `inner` is clicked.
