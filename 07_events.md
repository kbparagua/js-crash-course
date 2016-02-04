# 07: Events

## Adding Element Listener

```html
<a href="#" id="test">Click Me!</a>
```

A listener can be registered to an element by using the `addEventListener` method.

```js
var el = document.getElementById('test');

function listener(event){ alert("clicked."); }

el.addEventListener('click', listener);
```

Syntax:
```js
element.addEventListener(event, listener, triggerOnCapturePhase);
```

(*`triggerOnCapturePhase` argument will be discussed later.*)

### Multiple Listeners

```js
var el = document.getElementById('test');

el.addEventListener('click', function(e){ console.log('clicked - 1st'); });
el.addEventListener('click', function(e){ console.log('clicked - 2nd'); });
el.addEventListener('click', function(e){ console.log('clicked - 3rd'); });
```

When an element has multiple listeners, they will be triggered in order which they are added. In this case the output when the element is clicked is:

```
clicked - 1st
clicked - 2nd
clicked - 3rd
```

## The Event Object

When a listener is executed, an event object is passed to it, which contains details of the triggered event. Event properties may vary depending on the event type.

```js
el. addEventListener('click', function(event){
  
  // Access the coordinate where the event is triggered.
  event.clientX;
  event.clientY;
  
});
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

});
```

## Preventing Other Listeners Execution

Inside a listener, you can stop the next listeners from executing by invoking the event object method `stopImmediatePropagation()`.

```js
el.addEventListener('click', function(e){ console.log('clicked - 1st'); });

el.addEventListener('click', function(e){
  // Prevent next listeners from executing
  e.stopImmediatePropagation();
  console.log('clicked - 2nd');
});

el.addEventListener('click', function(e){ console.log('clicked - 3rd'); });
```

When element is clicked, only the 1st and 2nd listeners will be executed.

```
clicked - 1st
clicked - 2nd
```

## Propagation

When a event is triggered, that same event will also be triggered on the ancestors of the target element.

```html
<div id="parent">
  <div id="child"></div>
</div>
```

```js
var parent = document.getElementById('parent'),
    child = document.getElementById('child');
  
child.addEventListener('click', function(e){
  console.log('child is clicked');
});

parent.addEventListener('click', function(e){
  console.log('parent is clicked');
});
```

When child element is clicked:
```
child is clicked
parent is clicked
```

When parent element is clicked:
```
parent is clicked
```

## Propagation Cycle

When an event is triggered, the event will undergo 2 phases, capture and bubble phase.

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

When adding a listener you can only choose to execute it on capture phase or bubble phase, but not both.

### Capture Phase

By default, all listeners will execute on bubble phase. To execute a listener on capture phase, you need to pass `true` as the 3rd argument of `addEventListener`.

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
