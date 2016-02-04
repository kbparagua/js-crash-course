# 07: Events

## Listeners

```html
<a href="#" id="test">Click Me!</a>
```

A listener can be registered to an element by using the `addEventListener` method.

```js
var el = document.getElementById('test');

function listener(event){ alert("clicked."); }

el.addEventListener('click', listener, false);
```

Syntax:
```js
element.addEventListener(event, listener, triggerOnCapturePhase);
```

## Multiple Listeners

```js
var el = document.getElementById('test');

el.addEventListener('click', function(e){ console.log('clicked - 1st'); }, false);
el.addEventListener('click', function(e){ console.log('clicked - 2nd'); }, false);
el.addEventListener('click', function(e){ console.log('clicked - 3rd'); }, false);
```

When an element has multiple listeners, they will be triggered in order which they are added. In this case the output when the element is clicked is:

```
clicked - 1st
clicked - 2nd
clicked - 3rd
```

## Preventing Browser's Default Action

```html
<a href="http://www.google.com" id="my-link">Google</a>
```

In order to prevent the browser's default action, we just need to invoke the `preventDefault` method of the event object.

```js
var link = document.getElementById('my-link');

link.on('click', function(event){
  
  // Prevent browser from visiting the link's href value.
  event.preventDefault();

}, false);
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
