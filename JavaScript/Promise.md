# Promise
```
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```
## then
```
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
```

## catch
If we’re interested only in errors, then we can use null as the first argument: .then(null, errorHandlingFunction). Or we can use .catch(errorHandlingFunction), which is exactly the same:

## finally
It is a good handler to perform cleanup, e.g. to stop our loading indicators in finally, as they are not needed any more, no matter what the outcome is.

```
new Promise((resolve, reject) => {
  /* do something that takes time, and then call resolve/reject */
})
  // runs when the promise is settled, doesn't matter successfully or not
  .finally(() => stop loading indicator)
  .then(result => show result, err => show error)
```
It’s not exactly an alias though. There are several important differences:

* A finally handler has no arguments. In finally we don’t know whether the promise is successful or not. That’s all right, as our task is usually to perform “general” finalizing procedures.

* Finally passes through results and errors to the next handler.

# Promise chaining
.then() returns a thenable object:
```
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```
## using fetch
```
// Make a request for user.json
fetch('/article/promise-chaining/user.json')
  // Load it as json
  .then(response => response.json())
  // Make a request to github
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  // Load the response as json
  .then(response => response.json())
  // Show the avatar image (githubUser.avatar_url) for 3 seconds (maybe animate it)
  .then(githubUser => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => img.remove(), 3000); // (*)
  });
```
problems with `fetch`: https://medium.com/@shahata/why-i-wont-be-using-fetch-api-in-my-apps-6900e6c6fe78

http toolkits: axios

# promise error handling
* add .catch() to the end of promise chain, it will be called when any of the promise reject.

* any numbers of .catch() can add in any step in the promise chain, when err, the closest .catch() will be called.

* implicitly throw error equals rejecting promise:
```
new Promise((resolve, reject) => {
  throw new Error("Whoops!");
}).catch(alert); // Error: Whoops!
```
sometimes, we need to implicitly throw err and call .catch

* Unhandled rejections
if no .catch() can handle the error, most JavaScript engines track such situations and generate a global error in that case. We can see it in the console.

In the browser we can catch such errors using the event unhandledrejection:
```
window.addEventListener('unhandledrejection', function(event) {
  // the event object has two special properties:
  alert(event.promise); // [object Promise] - the promise that generated the error
  alert(event.reason); // Error: Whoops! - the unhandled error object
});

new Promise(function() {
  throw new Error("Whoops!");
}); // no catch to handle the error
```

## Summay
* .catch handles promise rejections of all kinds: be it a reject() call, or an error thrown in a handler.
* We should place .catch exactly in places where we want to handle errors and know how to handle them. The handler should analyze errors (custom error classes help) and rethrow unknown ones.
* It’s normal not to use .catch if we don’t know how to handle errors (all errors are unrecoverable).
* In any case we should have the unhandledrejection event handler (for browsers, and analogs for other environments), to track unhandled errors and inform the user (and probably our server) about the them. So that our app never “just dies”.

TODO: write a customized error handler
And finally, if we have load-indication, then .finally is a great handler to stop it when the fetch is complete
```
function demoGithubUser() {
  let name = prompt("Enter a name?", "iliakan");

  document.body.style.opacity = 0.3; // (1) start the indication

  return loadJson(`https://api.github.com/users/${name}`)
    .finally(() => { // (2) stop the indication
      document.body.style.opacity = '';
      return new Promise(resolve => setTimeout(resolve, 0)); // (*)
    })
    .then(user => {
      alert(`Full name: ${user.name}.`);
      return user;
    })
    .catch(err => {
      if (err instanceof HttpError && err.response.status == 404) {
        alert("No such user, please reenter.");
        return demoGithubUser();
      } else {
        throw err;
      }
    });
}

demoGithubUser();
```
Here on the line (1) we indicate loading by dimming the document. The method doesn’t matter, could use any type of indication instead.

When the promise is settled, be it a successful fetch or an error, finally triggers at the line (2) and stops the indication.

There’s a little browser trick (*) with returning a zero-timeout promise from finally. That’s because some browsers (like Chrome) need “a bit time” outside promise handlers to paint document changes. So it ensures that the indication is visually stopped before going further on the chain.

## implicitly throw error won't work for asychronous code
```
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("Whoops!");
  }, 1000);
}).catch(alert); // won't work

new Promise(function(resolve, reject) {
  setTimeout(() => {
    reject('error')
  }, 1000);
}).catch(alert); // work
```

# Promise API
## Promise.resolve
```
let promise = Promise.resolve(value);
```
same as:
```
let promise = new Promise(resolve => resolve(value));
```

## Promise.reject
## Promise.all
```
Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(alert); // 1,2,3 when promises are ready: each promise contributes an array member
```
If any of the promises is rejected, Promise.all immediately rejects with that error.
```
Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).catch(alert); // Error: Whoops!
```
Here the second promise rejects in two seconds. That leads to immediate rejection of Promise.all, so .catch executes: the rejection error becomes the outcome of the whole Promise.all.

The important detail is that promises provide no way to “cancel” or “abort” their execution. So other promises continue to execute, and the eventually settle, but all their results are ignored.

There are ways to avoid this: we can either write additional code to clearTimeout (or otherwise cancel) the promises in case of an error, or we can make errors show up as members in the resulting array (see the task below this chapter about it).

## Promise.race(promises)
waits for the first promise to settle, and its result/error becomes the outcome.

## Solution for Promise.all
We can’t change the way Promise.all works: if it detects an error, then it rejects with it. So we need to prevent any error from occurring. Instead, if a fetch error happens, we need to treat it as a “normal” result.

Here’s how:
```
Promise.all(
  fetch('https://api.github.com/users/iliakan').catch(err => err),
  fetch('https://api.github.com/users/remy').catch(err => err),
  fetch('http://no-such-url').catch(err => err)
)
```

# Promisification
Promisification – is a long word for a simple transform. It’s conversion of a function that accepts a callback into a function returning a promise.

**note**
Promisification is a great approach, especially when you use async/await (see the next chapter), but not a total replacement for callbacks.

Remember, a promise may have only one result, but a callback may technically be called many times.

So promisification is only meant for functions that call the callback once. Furhter calls will be ignored.

# microtask and macrotask
**Asynchronous tasks** need proper management. For that, the standard specifies an internal queue PromiseJobs, more often called the **“microtask queue”**.
* The queue is first-in-first-out: tasks that get enqueued first are run first.
* Execution of a task is initiated only when nothing else is running.

when events happen, **events** will be pushed to a **"macrotask queue"**.
Events example:
* setTimeout
* mouseover
* fetch

**Microtask queue has a higher priority than the macrotask queue.**

nothing else is runnning => run microtasks => run macrotasks

# Async/await
So, async ensures that the function returns a promise, and wraps non-promises in it.
```
async function f() {
  return 1;
}

f().then(alert); // 1
```

The keyword await makes JavaScript wait until that promise settles and returns its result.

Here’s an example with a promise that resolves in 1 second:
```
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // wait till the promise resolves (*)

  alert(result); // "done!"
}

f();
```
The function execution “pauses” at the line (*) and resumes when the promise settles, with result becoming its result. So the code above shows “done!” in one second.

Let’s emphasize: await literally makes JavaScript wait until the promise settles, and then go on with the result. That doesn’t cost any CPU resources, because the engine can do other jobs meanwhile: execute other scripts, handle events etc.

It’s just a more elegant syntax of getting the promise result than promise.then, easier to read and write.

# async generator
So far we’ve seen simple examples, to gain basic understanding. Now let’s review a real-life use case.

There are many online APIs that deliver paginated data. For instance, when we need a list of users, then we can fetch it page-by-page: a request returns a pre-defined count (e.g. 100 users), and provides an URL to the next page.

The pattern is very common, it’s not about users, but just about anything. For instance, Github allows to retrieve commits in the same, paginated fasion:

* We should make a request to URL in the form https://api.github.com/repos/<repo>/commits.
* It responds with a JSON of 30 commits, and also provides a link to the next page in the Link header.
* Then we can use that link for the next request, to get more commits, and so on.
What we’d like to have is an iterable source of commits, so that we could use it like this:
```
let repo = 'iliakan/javascript-tutorial-en'; // Github repository to get commits from

for await (let commit of fetchCommits(repo)) {
  // process commit
}
```
We’d like fetchCommits to get commits for us, making requests whenever needed. And let it care about all pagination stuff, for us it’ll be a simple for await..of.

With async generators that’s pretty easy to implement:
```
async function* fetchCommits(repo) {
  let url = `https://api.github.com/repos/${repo}/commits`;

  while (url) {
    const response = await fetch(url, { // (1)
      headers: {'User-Agent': 'Our script'}, // github requires user-agent header
    });

    const body = await response.json(); // (2) parses response as JSON (array of commits)

    // (3) the URL of the next page is in the headers, extract it
    let nextPage = response.headers.get('Link').match(/<(.*?)>; rel="next"/);
    nextPage = nextPage && nextPage[1];

    url = nextPage;

    for(let commit of body) { // (4) yield commits one by one, until the page ends
      yield commit;
    }
  }
}
```
1. We use the browser fetch method to download from a remote URL. It allows to supply authorization and other headers if needed, here Github requires User-Agent.
2. The fetch result is parsed as JSON, that’s again a fetch-specific method.
3. We can get the next page URL from the Link header of the response. It has a special format, so we use a regexp for that. The next page URL may look like this: https://api.github.com/repositories/93253246/commits?page=2, it’s generatd by Github itself.
4. Then we yield all commits received, and when they finish – the next while(url) iteration will trigger, making one more request.
An example of use (shows commit authors in console):
```
(async () => {

  let count = 0;

  for await (const commit of fetchCommits('iliakan/javascript-tutorial-en')) {

    console.log(commit.author.login);

    if (++count == 100) { // let's stop at 100 commits
      break;
    }
  }

})();
```

TODO: performance