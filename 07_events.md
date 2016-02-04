# 07: Events

## Adding Event Listener

An event listener can be registered to an element by using the `addEventListener` method.

```html
<a href="#" id="test">Click Me!</a>
```

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

## Event Object

When a listener is executed, an event object is passed to it, which contains details of the triggered event.

```js
el.addEventListener('click', function(event){
  // Access the coordinate where the event is triggered.
  event.clientX;
  event.clientY;
  
  // The element that triggered the event.
  event.target;
});
```

## `this` Value

When a listener is executed `this` will refer to the element object where it is attached.

```js
element.addEventListener('click', function(event){
  console.log(this === element); // true
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

Inside a listener, you can stop the next listeners from executing by invoking the event object method `stopImmediatePropagation`.

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

```html
<div id="a">
  <div id="b">
    <div id="c">
      <div id="d">
      </div>
    </div>
  </div>
</div>
```

```js
var a = document.getElementById('a'),
    b = document.getElementById('b'),
    c = document.getElementById('c'),
    d = document.getElementById('d');

a.addEventListener('click', function(e){ console.log('a'); }, true);
b.addEventListener('click', function(e){ console.log('b'); });
c.addEventListener('click', function(e){ console.log('c'); }, true);
d.addEventListener('click', function(e){ console.log('d'); });
```

When the inner-most child is clicked, `d`, output will be:

```
a (capture phase)
c (capture phase)
d (bubble phase)
b (bubble phase)
```

## Stopping Propagation

In order to stop event propagation, either on capture or bubble phase, simply invoke the `stopPropagation` method of the event object.

```html
<div id="a">
  <div id="b">
    <div id="c">
      <div id="d">
      </div>
    </div>
  </div>
</div>
```

```js
a.addEventListener('click', function(e){ console.log('a'); });

b.addEventListener('click', function(e){ 
  e.stopPropagation();
  console.log('b'); 
});

c.addEventListener('click', function(e){ console.log('c'); });
d.addEventListener('click', function(e){ console.log('d'); });
```

When `d` element is clicked:

```
d
c
b // propagation stopped
```

When `c` element is clicked:

```
c
b // propagation stopped
```

**NOTE:**
`stopImmediatePropagation` can also be used to stop the propagation, just keep in mind that besides stopping propagation, it will also prevent other listeners of the current element from executing.

`stopPropagation` will just stop propagating the event to the ancestors (bubble phase) or children (capture phase), but it will still execute other listeners of the current element that handles the event.

```html
<div id="outer">
  <div id="inner"></div>
</div>
```

```js
var outer = document.getElementById('outer'),
    inner = document.getElementById('inner');
    

outer.addEventListener('click', function(e){ console.log('outer'); });

inner.addEventListener('click', function(e){
  e.stopPropagation();
  console.log('inner');
});

inner.addEventListener('click', function(e){
  console.log('inner 2');
});
```

When `inner` element is clicked:

```
inner (propagation will stop, but will still execute the other listener)
inner 2
```

## Removing Event Listener

To remove an event listener, the same arguments used during adding it should be passed again to the method `removeEventListener`.

```js
var listener = function(e){ console.log('triggered'); };

el.addEventListener('click', listener, true);
el.removeEventListener('click', listener, true);
```

Passing a copy of the added listener will not work, the same exact listener object should be passed.

```js
el.addEventListener('click', function(e){ console.log('click'); });
el.removeEventListener('click', function(e){ console.log('click'); }); // will not remove the original listener.
```
