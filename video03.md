# Enforce a null check with composable code branching using Either
[Video](https://egghead.io/lessons/javascript-composable-code-branching-with-either)

Allows for pure functional error handling, code branching, null checking, e.g. anything that can be captured in a disjunction (or).

```js
// const Either = Right || Left

const Right = x =>
({
  map: f => Right(f(x)),
  fold: (f, g) => g(x), // fold to right
  inspect: () => `Right(${x})`,
})
console.log(Right(3).map(x => x + 1).map(x => x / 2)) // -> Right(2)

const Left = x =>
({
  map: f => Left(x), // left ignores f()
  fold: (f, g) => f(x), // fold to left
  inspect: () => `Left(${x})`,
})
console.log(Left(3).map(x => x + 1).map(x => x / 2)) // -> Left(3)

console.log(Right(2).map(x => x + 1).map(x => x / 2).fold(x => 'error', x => x) // -> 1.5
console.log(Left(2).map(x => x + 1).map(x => x / 2).fold(x => 'error', x => x) // -> error

// find color example
const fromNullable = x => x !== null ? Right(x) : Left(null)

// findColor needs to guard against color not found
const findColor => name => fromNullable(({red: '#ff4444', blue: '#3b5998', yellow: '#fff68f'})[name])

const processColor = color => color
  .map(c => c.slice(1))
  .fold(
    e => 'no color',
    c => c.toUpperCase(),
  )

console.log(processColor(findColor('red'))) // -> FF4444
console.log(processColor(findColor('green'))) // -> no color

```
