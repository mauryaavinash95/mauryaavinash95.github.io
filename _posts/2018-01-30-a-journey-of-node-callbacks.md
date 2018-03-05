---
layout: post
title:  A journey of Node Callbacks
date:   2018-01-30 02:00:24 +0530 
categories: NodeJS
comments: true
tags: Nodejs, V8, Callbacks
---
After scratching my head for a while on one of my colleague's question, I decided to start understanding the way nodejs works, how it does what it does and how has it become different than other runtimes out there.
The question.      
> Consider there are 3 requests hitting the node server, namely R1, R2 and R3 consequently, so these requests are stacked up as R1,R2,R3 with R1 being at the bottom of the stack and R3 being at the top. And these requests are to three different services (I/O calls). What would happen if R2 gets completed first, would it wait for the R3 to pop or node would deal with it some other way?

### Twas' a tricky one      
It was then that I found [this awesome talk of Philip Roberts at JS conf 2015](https://www.youtube.com/watch?v=8aGhZQkoFbQ). It was just the best explanation of how things work around in there. This post would be an attempt to reiterate it as simply as possible.

### What are the things which come into picture?        
1. **Call Stack**: This is the place where all our executions get first called up at. If there's a thing which the V8 Engine knows how to do, or native calls, it gets handled in this arena. Things like console, for/while loops, delays etc..      
So what about things like setTimeout, fetch or other async operations? They are not a part of V8, they are browser features and hence get handled in a different part called as the Web APIs.       
2. **Web APIs**: It contains a majority of things, mostly javascript which is responsible for our async operations, say fetch or setTimeout or setInterval.         
3. **Callback Queue**: This is the place where actual magic happens, as soon as the web-apis finish their task, they trigger out their result to the callback queue. It's the responsibility of this callback queue then to send the result to call-stack, in FIFO order.       
4. **Renderer Queue**: Since JS is single threaded, there are a lot of things which the lone main thread has to do, from rendering css, doing layout to computation and a lot in between. The renderer queue is responsible for showing up the changes to the DOM which occur. So if there's something occupying up the call-stack, the rendering gets blocked, till the time call-stack is empty.      

### Understanding this step by step.
1. You call any async function, say `setTimeout()`, this isn't something which belongs for execution in the call-stack, so the call stack would callup the web-api and transfer the processing of this setTimeout to it.            
2. The web-api would allocate a new thread for execution of the process from step-1 from the thread pool.       
3. As soon as the task gets completed, the completed result is sent back to the callback queue.       
4. Whenever the call-stack is empty, it picks up objects from the callback queue from the front (FIFO) and uses the result for whatever next computation it requires it.    

Diagramatically,   
![Diagramatic Relation]({{site.baseurl}}/images/a-journey-of-node-callbacks/NodeJSCallback-1.png)  


So to answer the above question, as soon as the responses (RS1, RS2, RS3) return to the callback queue, it would take them up in FIFO fashion and pass on to the main call-stack.  

### The magic behind all of this: Event Loop       
At the start of any node process, it creates a so called event loop, which consists of 6 major phases: 
1. Timers
2. I/O callbacks
3. Idle/prepare 
4. Poll
5. Check
6. Close callbacks

##### Timers        
These are the components like `setTimeout()` & `setInterval()`. An alarm for these events are stacked up and as soon as they go off, the polling phase recognizes it and goes and executes the callback of the given function.      

##### I/O Callbacks     
These are the network/system callbacks which get queued. Eg. `ECONNREFUSED` while connecting to some socket will be queued and will get executed in I/O Callback phase.       
##### Poll  
The poll serves two purposes: checking the timers and processing events in poll queue. The polling continues till the time all callbacks in the poll queue have been completed or there are active timers in the queue.         

##### Check         
In this phase, we check if poll phase is idle using the `setImmidiate()` function. The check phase is scheduled as soon as it finds poll phase in idle state.       

##### Close Callbacks
If a socket or handle is closed abruptly (e.g. `socket.destroy()`), the 'close' event will be emitted in this phase. Otherwise it will be emitted via `process.nextTick()`.

It can be all summed up something like this:    
```
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘
```
[Image Source](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

> Stay tuned for more...


