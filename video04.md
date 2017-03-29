# Use chain for composable error handling with nested Eithers
[Video](https://egghead.io/lessons/javascript-composable-error-handling-with-either)

```js
// const Either = Right || Left

const Right = x =>
({
  chain: f => f(x),
  map: f => Right(f(x)),
  fold: (f, g) => g(x),
  inspect: () => `Right(${x})`,
})

const Left = x =>
({
  chain: f => Left(x),
  map: f => Left(x),
  fold: (f, g) => f(x),
  inspect: () => `Left(${x})`,
})

const fromNullable = x => x !== null ? Right(x) : Left(null)

import fs from 'fs'

const getPort = () => {
  try {
    const str = fs.readFileSync('config.json')
    const config = JSON.parse(str)
    return config.port
  } catch(e) {
    return 3000
  }
}

const tryCatch = f => {
  try {
    return Right(f())
  } catch(e) {
    return Left(e)
  }
}

const getPort = () =>
  tryCatch(() => fs.readFileSync('config.json'))
  .chain(c => tryCatch(() => JSON.parse(c))) // tryCatch returns a Right(Right) or a Right(Left), chain unfolds it to either Right or Left
  .fold(e => 3000, c => c.port)
```
