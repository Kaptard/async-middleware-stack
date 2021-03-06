# async-middleware-stack
Simple middleware controller. Like express, but async.

<br>

## Usage
```js
const Stack = require('async-middleware-stack')
const stack = new Stack()

// Add middleware function to the stack
stack.use(async (req, res) => {
  await new Promise(resolve => setTimeout(resolve, 5000)) // Wait for 5s
  console.log('Praise kek')
})

// Run the stack
const req = { /** usual node.js req object **/ }
const res = { /**           ^^^^           **/ }
const passed = await stack.run(req, res)

if (passed) {
  // Continue if middleware stack passed
}
```
To break the stack chain, simply return any truthy value. `stack.run` will then
return `false`.

<br>

## API

### new Stack(config)
> Returns a Stack controller.

(optional) `config` options:

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| middleware | Existing array of functions to base the stack on  | None |

<br>

### stack.use(route, fn, method)
> Adds a function to the middleware stack.

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| route | URL to limit the function to. If first arg is a function, it'll be considered as `fn`, **NOT** as `route`. | `'*'` |
| fn | Function to execute with `req`, `res`, `opitonal` objects. | None |
| fn | RESTful method to limit the function to. Default accepts any. | `'*'` |

<br>

### stack.run(req, res, optional)
> Returns a promise resolving with `true` when all functions are done running

| Argument | Description | Default |
|:------------- |:------------- |:------------- |
| req | Request object from node.js/express. Must have `url` property. | None |
| res | Response object from node.js/express. | None |
| optional | Optional object to pass to every function. | None |

<br>
