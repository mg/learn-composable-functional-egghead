# Create linear data flow with container style types (Box)
[Video](https://egghead.io/lessons/javascript-linear-data-flow-with-container-style-types-box?course=professor-frisby-introduces-composable-functional-javascript)

- ``map`` is more about **composition** within a context than **iteration**. In this case, **box** is our context.

```js
// original function
const nextCharForNumberString = str => {
  const trimmed = str.trim()
  const number = parseInt(str)
  const nextNumber = number + 1
  return String.fromCharCode(number)
}

// compact
const nextCharForNumberString = str =>
  String.fromCharCode(parseInt(str.trim()) + 1)

// boxing for linear flow
const nextCharForNumberString = str =>
  [str] // create the box
  .map(s => s.trim())
  .map(s => parseInt(s))
  .map(i => i + 1)
  .map(i => String.fromCharCode(i))
  .map(i => i[0]) // unbox

// more formal way, Box is the identity functor
const Box = x =>
  ({
    map: f => Box(f(x)), // return a box so we can chain
    fold; f => f(x),
    inspect: () => `Box(${x})`,
  })

const nextCharForNumberString = str =>
  Box(str) // create the box
  .map(s => s.trim())
  .map(s => parseInt(s))
  .map(i => i + 1)
  .fold(i => String.fromCharCode(i)) // unbox

```
