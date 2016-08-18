# async-parallel
Simplifies invoking tasks in parallel using TypeScript async/await.

This package provides the following functions:
* **Parallel.each** calls a provided function once per input in parallel   
* **Parallel.map** creates a new array with the results of calling a provided function in parallel on every input, the order of the outputs will correspond to the inputs
* **Parallel.pool** creates a fixed size pool of workers fetching tasks until a false result is obtained, and returns when the pool is drained to zero   
* **Parallel.invoke** calls a set of provided functions in parallel

## Parallel.each example
```js
var list = [100, 200, 300]; // provide list of inputs here
await Parallel.each(list, async (item) => {
    // process each item here
});
```

## Parallel.map example
```js
var list = [100, 200, 300]; // provide list of inputs here
var result = await Parallel.map(list, async (item) => {
    // process each item here
});
// result available here
```


## Parallel.pool example
```js
// pool size of 2 with 3 tasks (no more than 2 tasks running at a time)
var tasks = [
    async function (): Promise<void> { /* task #0*/ },
    async function (): Promise<void> { /* task #1*/ },
    async function (): Promise<void> { /* task #2*/ }
];
await Parallel.pool(2, async () => {
    var task = tasks.shift();
    await task();
    return tasks.length > 0;
});
// all tasks complete
```



## Parallel.invoke example
```js
await Parallel.invoke([
    async function (): Promise<void> {
        // first action here
    },
    async function (): Promise<void> {
        // second action here
    },
    async function (): Promise<void> {
        // third action here
    }
}); 
```

## Getting Started

Make sure you're running Node v4 and TypeScript 1.8 or higher...
```
$ node -v
v4.2.6
$ npm install -g typescript
$ tsc -v
Version 1.8.10
```

Install package...
```
$ npm install async-parallel
```

Write some code...
```js
import * as Parallel from 'async-parallel';
(async function () {
    var list = [100, 200, 300];
    var start = new Date();
    await Parallel.each(list, async (value) => {
        await sleep(value);
        console.log('sleep', value);
    });
    console.log('done', new Date().getTime() - start.getTime());
    function sleep(ms: number) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
})();
```

Save the above to a file (index.ts), build and run it!
```
$ tsc index.ts --target es6 --module commonjs
$ node index.js
sleep 100
sleep 200
sleep 300
done 303
```

## Additional Notes
Setting Parallel.concurrency=1 causes all iterations to be performed in series to facilitate debugging/troubleshooting.
