# A collection of Either examples compared to imperative code
[Video](https://egghead.io/lessons/javascript-a-collection-of-either-examples-compared-to-imperative-code)

```js
// open site
const openSite = () => {
  if(current_user) {
    return renderPage(current_user)
  } else {
    return showLogin()
  }
}

const openSite = () =>
  fromNullable(current_user)
  .fold(showLogin, renderPage)

// get preferences
const getPrefs = user =>
  if(user.premium) {
    return loadPrefs(user.preferences)
  } else {
    return defaultPrefs
  }
}

const getPrefs = user =>
  (user.premium ? Right(user) : Left('not premium'))
  .map(u => u.preferences)
  .fold(() => defaultPrefs, prefs => loadPrefs(prefs))

// street name
const streetName = user => (
  const address = user.address
  if(address) {
    const street = address.street
    if(street) {
      return street.name
    }
  }
  return 'no street'
)

const streetName = user =>
  fromNullable(user.address)
  .chain(a => fromNullable(a.street))
  .map(s => s.name)
  .fold(e => 'no street', n => n)

// concat uniq
const concatUniq = (x, ys) => {
  const found = ys.filter(y => y === x)[0]
  return found ? ys : ys.concat(x)
}

const concatUniq = (x, ys) =>
  fromNullable(ys.filter(y => y === x)[0])
  .fold(() => ys.concat(x) : () => ys)

// wrap examples
const wrapExamples = example => {
  if(example.previewPath) {
    try {
      example.preview = fs.readFileSync(example.previewPath)
    } catch(e) {}
  }
  return example
}

const readFile = x => tryCatch(() => fs.readFileSync(x))

const wrapExamples = example =>
  fromNullable(example.previewPath)
  .chain(readFile)
  .fold(
    () => example,
    ex => Object.assign({preview: p}, ex),
  )

// parse db url
const parseDbUrl = cfg => {
  try {
    const c = JSON.parse(cfg)
    if(c.url) {
      return c.url.match(/postgres: .../)
    }
  } catch(e) {
    return null
  }
}

const parseDbUrl = cfg =>
  tryCatch(() => JSON.parse(cfg))
  .chain(c => fromNullable(c.url))
  .fold(
    e => null,
    u => u.match((postgres: .../))
  )
```
