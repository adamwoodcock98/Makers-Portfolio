### Using fetch

* Fetch allows us to retrieve saved data from a server
* Fetch creates a route sending a HTTP request
* A promise is now ‘pending'
  * A promise is like a handle for the data
  * A promise may take time to come back depending on the connection
* The HTTP response is received
* The promise state moves to ‘fulfilled'
* We can then use `​.then`​ to access a callback with the response once this has been returned
* Now we can `​//do stuff`​ within the callback

We first encountered asynchronous programming when we looked at callbacks. Promises are also asynchronous, but a more modern way of working than with callbacks, they help to avoid messy nested callbacks that are very difficult to read.

When a promise moves to pending, the promise can fail, or be ‘rejected’ from this point. We can build handlers to deal with a rejection and action this in our code. It’s important that when we mock our API tests that we cover both successful, and unsuccessful responses.

The basic syntax for a fetch looks like this:

```js
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```

There is quite a lot happening here that it’s important to understand:

* When we call `​fetch`​ we are creating a promise
  * This could take some time to execute, which is why a promise is created
  * The promise resolves with a response object.
  * The response object does not directly contain the actual JSON response body, but is instead a representation of the entire HTTP response.
* In our first `​.then`​ call, we are converting our response, `​response.json()`​
  * This process can also take some time, especially if it’s a large file, so a second promise is created at this point
  * The `​.json()`​ method returns a second promise that resolves with the result of parsing the response body text as JSON.
* When promises are created, they need to finish before moving onto the next `​.then`​, only when they’ve finished, will they move on.
* Our second `​.then`​ call is passing us our data, we can do whatever we like with this, so this one does not create