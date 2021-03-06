## What do you know about promises?

The Promise object is used for asynchronous computations. A Promise represents a value which may be available now, or in the future, or never.

```js
const p = new Promise((resolve, reject) => {

  // Do an async task async task and then...
  if(/* good condition */) {
    resolve('Success!')
  } else {
    reject('Failure!')
  }
})

p.then(() => {
    /* do something with the result */
}).catch(() => {
    /* error :( */
})
```

Promises gives you an ability to perform deferred computations on values that are not in place yet.

A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers to an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise for the value at some point in the future.

A Promise is in one of these states:

- `pending`: initial state, not fulfilled or rejected.
- `fulfilled`: meaning that the operation completed successfully.
- `rejected`: meaning that the operation failed.

A `pending` promise can either be `fulfilled` with a value, or `rejected` with a reason (error). When either of these happens, the associated handlers queued up by a promise's `then` method are called. (If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there is no race condition between an asynchronous operation completing and its handlers being attached.)

As the `Promise.prototype.then()` and `Promise.prototype.catch()` methods return promises, they can be chained.
`Promise.prototype.catch()` behaves the same as calling `Promise.prototype.then(undefined, onRejected)`.

```js
doSomething().then(function () {
    return doSomethingElse()
}).then(finalHandler)

doSomething().then(function () {
    doSomethingElse()
}).then(finalHandler)

doSomething().then(doSomethingElse()).then(finalHandler)

doSomething().then(doSomethingElse).then(finalHandler)
```


1.

```
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

2.

```
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                  finalHandler(undefined)
                  |------------------|
```

3.
```
doSomething
|-----------------|
doSomethingElse(undefined)
|---------------------------------|
                  finalHandler(resultOfDoSomething)
                  |------------------|
```

4.
```
doSomething
|-----------------|
                  doSomethingElse(resultOfDoSomething)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

### Error catching gotchas

```js
// Errors thrown inside asynchronous functions will act like uncaught errors
var p2 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    throw 'Uncaught Exception!'
  }, 1000)
})

p2.catch(function(e) {
  console.log(e) // This is never called
})

// Errors thrown after resolve is called will be silenced
var p3 = new Promise(function(resolve, reject) {
  resolve()
  throw 'Silenced Exception!'
})

p3.catch(function(e) {
   console.log(e) // This is never called
})
```
