# 07: Events


## Propagation Cycle

1. During the capture phase, the farthest ancestor will be visited first, down to the target element.
2. During the bubble phase, the target element will be visited first, up to the farthest ancestor.

```js
//   ______________________
//  |element_1             |  ||            /\
//  |   _______________    |  ||            ||
//  |  |element_2      |   |  || Capture    || 
//  |  |               |   |  ||            || Bubble
//  |  |_______________|   |  ||            ||
//  |______________________|  \/            ||
// 
```

**Capture Phase** order:
  1. `element_1`
  1. `element_2`

**Bubble Phase** order:
  1. `element_2`
  1. `element_1`
