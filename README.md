# nanocomponent [![stability][0]][1]
[![npm version][2]][3] [![build status][4]][5] [![test coverage][6]][7]
[![downloads][8]][9] [![js-standard-style][10]][11]

Create performant HTML elements.

## Features
- works with every framework
- significantly speeds up perceived performance
- improves rendering performance
- graciously falls back if APIs are not available
- weighs `~2kb`

## Usage
### Cache a static HTML element
```js
var component = require('nanocomponent')
var html = require('bel')

var staticElement = component(html`
  <div>heya</div>
`)
var el = staticElement()
console.log(el)
```

### Cache a dynamic HTML element
```js
var cachedElement = component(function (foo) {
  return html`
    <div>${foo}</div>
  `
})
console.log(cachedElement('hey folks'))   // render
console.log(cachedElement('hey folks'))   // return cached element
console.log(cachedElement('hey humans'))  // render again
```

### Defer rendering until the browser has spare time available
```js
var politeElement = component({
  placeholder: function () {
    return html`
      <p>lol not loaded yet</p>
    `
  },
  render: function () {
    return html`
      <p>HOW'S IT GOING CHAP</p>
    `
  }
})

console.log(politeElement())
setTimeout(function () {
  console.log(politeElement())
}, 1000)
```

### Trigger lifecycle events when loading and unloading elements to the DOM
```js
var widgetElement = component({
  onload: function (el) {
    el.textContent = 'totally loaded now'
  },
  onunload: function (el) {
    el.textContent = 'no more free lunch'
  },
  onupdate: function (el, verb) {
    el.textContent = `totally ${verb}ing now`
  },
  render: function (verb) {
    return html`
      <p>lol not ${verb}ed yet</p>
    `
  }
})
console.log(widgetElement('load'))
var el = widgetElement('blep')
document.body.appendChild(el)
document.body.removeChild(el)
```

### Trigger lifecycle events when coming in and out of view
```js
var viewportElement = component({
  onenter: function (el) {
    el.textContent = 'BEHOLD THE GOBLIN'
  },
  onexit: function (el) {
    el.textContent = 'THE PONIES HAVE COME'
  },
  render: function () {
    return html`
      <h1>WHO COULD IT BE</h1>
    `
  }
})
console.log(viewportElement())
```

## API
### render = nanocomponent(HtmlOrFunctionOrObject)
Create a render function for a component based depending on the arguments that
are passed in:
- __HTMLElement:__ when a valid HTML node is passed
- __function:__ cache the result of the function until new arguments are passed
- __object:__ create an object with different methods attached

### el = render(...args)

## Installation
```sh
$ npm install nanocomponent
```

## See Also
- [yoshuawuyts/nanomorph](https://github.com/yoshuawuyts/nanomorph)
- [yoshuawuyts/nanotick](https://github.com/yoshuawuyts/nanotick)
- [yoshuawuyts/nanoraf](https://github.com/yoshuawuyts/nanoraf)
- [bendrucker/document-ready](https://github.com/bendrucker/document-ready)
- [shama/on-load](https://github.com/shama/on-load)

## License
[MIT](https://tldrlegal.com/license/mit-license)

[0]: https://img.shields.io/badge/stability-experimental-orange.svg?style=flat-square
[1]: https://nodejs.org/api/documentation.html#documentation_stability_index
[2]: https://img.shields.io/npm/v/nanocomponent.svg?style=flat-square
[3]: https://npmjs.org/package/nanocomponent
[4]: https://img.shields.io/travis/yoshuawuyts/nanocomponent/master.svg?style=flat-square
[5]: https://travis-ci.org/yoshuawuyts/nanocomponent
[6]: https://img.shields.io/codecov/c/github/yoshuawuyts/nanocomponent/master.svg?style=flat-square
[7]: https://codecov.io/github/yoshuawuyts/nanocomponent
[8]: http://img.shields.io/npm/dm/nanocomponent.svg?style=flat-square
[9]: https://npmjs.org/package/nanocomponent
[10]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square
[11]: https://github.com/feross/standard
