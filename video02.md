# Refactor imperative code to a single composed expression using Box
[Video](https://egghead.io/lessons/javascript-refactoring-imperative-code-to-a-single-composed-expression-using-box)

- ``map`` is more about **composition** within a context than **iteration**. In this case, **box** is our context.

```js
const Box = x =>
  ({
    map: f => Box(f(x)),
    fold; f => f(x),
    inspect: () => `Box(${x})`,
  })

const moneyToFloat = str => parseFloat(str.replace(/\$/g, ''))

const percentToFloat = str => {
  const replaced = str.replace(/\%/g, '')
  const number = parseFloat(replaced)
  return number * 0.01
}

const applyDiscount = (price, discount) => {
  const cost = moneyToFloat(price)
  const savings = percentToFloat(discount)
  return cost - cost * savings
}

// rewrite with Box
const moneyToFloat = str =>
  Box(str.replace(/\$/g, ''))
  .map(s => parseFloat(s)) // keeping the box for composition in applyDiscount()

const percentToFloat = str =>
  Box(str.replace(/\%/g, ''))
  .map(s => parseFloat(s))
  .map(i => i * 0.01) // keeping the box for composition in applyDiscount()

const applyDiscount = (price, discount)
  moneyToFloat(price)
  .fold(cost => // use closure to capture cost for later calculation
      percentToFloat(discount)
      .fold(savings => cost - cost * savings) // savings comes through the map chain, cost through the closure
  )

const result = applyDiscount('$5.00', '20%')
```
