# async-middleware-stack
Simple middleware controller. Like express, but async.

<br>

## Usage
```js
const Stack = require('async-middleware-stack')
const stack = new Stack()

// Add middleware function to the stack
stack.use((req, res) => {
  return new Promise(resolve => {
    setTimeout(resolve, 5000)
  })
})

// Run the stack
const req = { /** usual node.js req object **/ }
const res = { /**           ^^^^           **/ }
try {
  await stack.run(req, res)
} catch (err) {
  // middleware chain broken, so you prolly don't wanna proceed.
}

```
To break the stack chain, simply throw a new error or reject a promise inside the
middleware function.

<br>

## API

### new Stack(config)
> Returns a Stack controller.

(optional) `config` options:

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| middleware | Existing array of functions to base the stack on  | None |

### stack.use(route, fn, method)
> Returns a promise resolving the return value of the given function.

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| route | URL to limit the function to. If first arg is a function, it'll be considered as `fn`, **NOT** as `route`. | `'*'` |
| fn | Function to execute with `req`, `res`, `opitonal` objects. | None |
| fn | RESTful method to limit the function to. Default accepts any. | `'*'` |

### stack.run(req, res, optional)
> Returns a promise resolving the return value of the given function.

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| req | Request object from node.js/express. Must have `url` property. | None |
| res | Response object from node.js/express. | None |
| optional | Optional object to pass to every function. | None |

<br>