# Advanced Nodejs Notes

> Nodejs -> v8(interpretation) + libuv(concurrency, async, event-loop)

- Every executable running no matter if it is foreground or background has to have a process.

- A process is the fundamental unit of execution in terms of a standalone application.

- You cannot have a single thread of something, a process should be there which can spawn multiple threads.

- Process is the parent entity which manages like shared memory b/w multiple threads.

- Scheduler's job is to allocate fair share(not equal) of cpu time based on some priority algorithms to every
  thread.

- Modern Computers and OSes are capable of running threads more than their logical or physical cores in cpu(context switching).

- Nodejs is fundamentally single threaded but what you can do with it is not exactly single threaded.

- When you run a js file in nodejs a process is started and now has a thread and this process will only run on a single core.

- It uses OS's abilities to execute tasks simultaneously on different cores to have a bigger picture that work is being done parallelly and concurrently.

- Consider an example that your application running on node, calls two async tasks(network,fs,encryption or any other thing related), Now what happens is that this code gets interpreted by v8 and is sent to OS for execution, the thing to keep in mind is that nodejs is single threaded but not the OS so it schedules these tasks to different cores/threads with the help of algorithms and now once the execution gets completed the results of both processes will get back to v8 and then to nodejs, but is interpreted by node one by one. This whole thing gets done with the help of "event-loop".

- Nodejs supports async tasks with the help of how event-loop works, you can have sort-of "multi-threaded behaviour" without the language being multi-threaded.

- libuv in nodejs has something called "threadpool" which is just a block of space ready to get assigned to a process to do a job, the more the threadpool size will be the more no. of threads we can compute at a time with the help of cpu cores (hyperthreading also helps i.e., 1 Physical Core == 2 Logical Cores).

> ```js
> /* 
> value can be any integer but is advised to have less than or
> equal to logial core size i.e., no of threads.
> For exp -> 6 cores == 12 threads 
> */
> process.env.UV_THREADPOOL_SIZE = 4;
> ```

- We need event loop so that we does not run everything on the single thread at the same time because that will kill the interactivity of the web page and requests/processes.

- Once a function probably async(like setTimeout(web apis) or other in nodejs) get completed on os level it gets added to callback/event queue where it waits for the call stack to get empty so that it can get executed without blocking the running task.

---

## Request Animation Frame

- RequestAnimationFrame is also another queue which only calls the functions stored in it when browser is about to render the new "DOM" updates, so it is very slow as compared to callback/event queue. It's speed depend upon the refresh rate of your monitor.

---

## MicroTask Queue

> Promises work on micro-task queue.

- Event loop first checks the micro-task queue before the actual task queue.

```js
function scheduleWork() {
  setTimeout(() => console.log("First"), 0);
  setTimeout(() => console.log("Second"), 0);
  Promise.resolve().then(() => console.log("First from microtask"));
  Promise.resolve().then(() => console.log("Second from microtask"));
}

scheduleWork();
/*
 Output:
> First from microtask
> Second from microtask
> First
> Second
*/

// but qhat if we intentionally put promises inside task-queue
// instead of micro-task queue
function scheduleWork() {
  setTimeout(() => console.log("First"), 0);
  setTimeout(() => console.log("Second"), 0);
  stTimeout(() =>
    Promise.resolve().then(() => console.log("First from microtask"), 0)
  );
  setTimeout(() =>
    Promise.resolve().then(() => console.log("Second from microtask"))
  );
  setTimeout(() =>
    setTimeout(() =>
      Promise.resolve().then(() => console.log("Nested from microtask"))
    )
  );
}

scheduleWork();
/*
 Output:
> First
> Second
> First from microtask
> Second from microtask
> Nested from microtask
*/
```

- One thing to here is that "Event loop" will keep checking the micro-task queue for functions to call until its empty. But that is not the case with the task-queue it only checks for fucntions to call once per iteration and calls the top function only.

```js
function scheduleWork() {
  Promise.resolve().then(() => console.log("m1"));
  Promise.resolve().then(() => console.log("m2"));
  setTimeout(() => console.log("t1"), 0);
  setTimeout(() => {
    Promise.resolve().then(() => console.log("m3"));
    Promise.resolve().then(() => console.log("m4"));
    Promise.resolve().then(() => console.log("m5"));
    Promise.resolve().then(() =>
      Promise.resolve().then(() => console.log("m6"))
    );
  }, 0);
  setTimeout(() => console.log("t2"), 0);
  setTimeout(() => console.log("t3"), 0);
}

scheduleWork();

/*
Output:
> m1
> m2
> t1
> m3
> m4
> m5
> m6
> t2
> t3
*/
```

- Another point is that task-queue and micro-task-queue will always be checked if and only if "callstack" is empty.

- Another Example:

```js
function scheduleWork() {
  Promise.resolve().then(() => console.log("#1 m1"));
  Promise.resolve().then(() => console.log("#1 m2"));
  setTimeout(() => console.log("#1 t1"), 0);
  setTimeout(() => {
    Promise.resolve().then(() => console.log("#1 m3"));
    Promise.resolve().then(() => console.log("#1 m4"));
    Promise.resolve().then(() => console.log("#1 m5"));
    Promise.resolve().then(() =>
      Promise.resolve().then(() => console.log("#1 m6"))
    );
  }, 0);
  setTimeout(() => console.log("#1 t2"), 0);
  setTimeout(() => console.log("#1 t3"), 0);
  console.log("#1 Hello from main thread");
}

function scheduleWorkAgain() {
  Promise.resolve().then(() => console.log("#2 m1"));
  Promise.resolve().then(() => console.log("#2 m2"));
  setTimeout(() => console.log("#2 t1"), 0);
  setTimeout(() => {
    Promise.resolve().then(() => console.log("#2 m3"));
    Promise.resolve().then(() => console.log("#2 m4"));
    Promise.resolve().then(() => console.log("#2 m5"));
    Promise.resolve().then(() =>
      Promise.resolve().then(() => console.log("#2 m6"))
    );
  }, 0);
  setTimeout(() => console.log("#2 t2"), 0);
  setTimeout(() => console.log("#2 t3"), 0);
  console.log("#2 Hello from main thread");
}

scheduleWork();
scheduleWorkAgain();

/*
Output:
#1 Hello from main thread
#2 Hello from main thread
#1 m1
#1 m2
#2 m1
#2 m2
#1 t1
#1 m3
#1 m4
#1 m5
#1 m6
#1 t2
#1 t3
#2 t1
#2 m3
#2 m4
#2 m5
#2 m6
#2 t2
#2 t3
*/
```
