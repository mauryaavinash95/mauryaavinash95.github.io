---
layout: post
title:  How React works
date:   2018-01-10 21:42:24 +0530
categories: React
comments: true
tags: React, Frontend
---
After the launch of [React Fibre](https://reactjs.org/blog/2017/09/26/react-v16.0.html), there has been a lot of excitment about the *"Fibre"* part of it. I also wanted to have a peek in, and after some searches, I was able to grasp something out of it. So, here is a lil snippet of my understanding of it.

### Looking at the bigger picture   
React, as we know it, is composed of two major parts,   
1. **Renderer**: The part responsible for manipulation of DOM*(ReactJS)*, Application State*(React Native)* or VR*(ReactVR)*. Analogically speaking, we can understand it as a painter, who knows how to hold a brush, when to use a roller, when to use a spray, how to give strokes etc. He knows how to interact with that wall's layer. Similarly Renderer is just a worker, who knows how to interact with the DOM, how to change the components and a bunch of other cool stuff.
2. **Reconciler**: This is the part where *magic* happens. It's the thing we called *"Virtual DOM"*, the thing thats fast and great and abstracts all the intense stuff for us developers. For React 15 and earlier we called it as the *"Virtual DOM"(Speaking the web way)*, now it's called the *Stack Reconciler* and React 16 introduced us to the new *Fibre Reconciler*. Analogically, this is the Designer, who is responsible to instruct the painter what, when, where and how to paint. So he is the one who is familiar with the latest designs trends, could perceive how the owner would want it and manage the overall aesthetics. Reconciler does the same thing, it knows that the developer wants a change of state on receiving data or on click of a button and it interally works around this and gives a final picture to Renderer to change things on the DOM.

### Why do we need the Reconciler?
The purpose of making the reconciler as a seperate unit was reusablility of the core logic. It is because of this that the working, semantics and flow of React is pretty much similar in all ReactJS, React Native and React VR. In React 16, this whole reconciler is a ground-up rewrite, paving way for the Fibre's birth, while maintaining backward compatibility.

### What the Reconciler does?
#### Understanding this, the Stack Reconciler way:
The reconciler is responsible for comparing the previous state with the current state, it does so by maintaining trees. The trees of current state version and the next(working) state of the application are compared. Each element(Eg.`<CustomComponent>, <div>, <span>, <li>` for web) is considered as a node of the tree.    
All these nodes are connected to the `HostRoot` which drives them all, connected to this `HostRoot` is our `root` element, the `<div>` element we generally specify in our index.html file with id="root" or id="app".  
So now that the main parent is `HostRoot`, `root` element becomes its child.    
The elements are connected in a hierarchical order i.e. if `element_2` is referenced inside `element_1`, `element_1` is parent of `element_2` and `element_2` is child of `element_1`.  
Similar goes for the case of siblings i.e. if `element_3`, `element_4` and `element_5` are connected at this same horizontal level to a parent element `element_2`, then `element_3`, `element_4` and `element_5` are siblings and `element_2` is their parent.   

Diagramatically,   
![Diagramatic Relation]({{site.baseurl}}/images/how-react-works/relationship.png)   

Now consider there's a change in `element_2` and `element_3`. What the stack reconciler does is that it compares the current tree, with the new changes and as soon as it notices a difference between the currently compared element, say `element_2`, it goes on to update the DOM of that corresponding element. Only once that is done, it goes on to compute the changes in next element `element_3` and then repaints it in the DOM.    
This is due to the fact that JS is single threaded, there's only one worker process/thread to take care of computation and painting, and no matter how fast these computations and DOM manipulations happen, this inconsistency will always be there, the state of the application would be prone to inconsistency by this mechanism.  

Diagramatically,
![Stack-reconciler-inconsistencies]({{site.baseurl}}/images/how-react-works/stack-reconciler-inconsistent-updates.png)    


#### Enters Awesome-Fibre 
Now that we've got some of the mechanics and glitches of the stack reconciler, let's see what *Fibre* has to offer us.    
React Fibre basically operates on two phases, the reconciliation/render phase and the commit phase. The former one is interruptable, but the later one isn't. In the reconciliation/render part, it does its computation, checks for all the elements which need updates and tries to complete the working-tree. In the commit part, it transfers the title of current tree to the working-tree(updated tree). Only after the commit phase is done, it alters the DOM, so there is no UI inconsitency observed. Here it would check all the elements which need update, and only when those are done, it proceeds to commit phase.  

##### How does it do that?
So what fibre does is it schedules the tasks based on priority, meaning the call-stack would now be alterable. The tasks with higher priorities are popped ahead on the call-stack as compared to the lower priority updates.       
*For example,  the change of button text would be a high priority update as compared to showing a response recieved from the server. The user would want as instantaneous update of his actions & interactions as possible.*  

Fibre takes advantage of the functionality of `requestIdleCallback` in modern browsers, or polyfills it in case its absent in that browser.   
So, there's our main thread, doing all the job of Javascript, CSS, DOM etc., but what fibre does using `requestIdleCallback` is it tells that main thread, *"Hey!, notify me whenever you're free, I got work for you."*. And as a sincere worker, the main thread does that, it goes to the *Fibre*, tells it the time it has to spare, say something like *"Boss, I got 16ms spare time now"*. So the reconciler uses this time to do the computations, creating the `working-tree` and `effect-list`.   

After 6ms of working, let's say there comes an urgent update(high priority update, say change of button text on clicking it), the main-thread won't go there instantly, it still has 10ms remaining of those 16ms it committed to the *Fibre*, so it walks along for those 10ms with the task at hand and then returns to acknowlegde that urgent update. 

It completes working on that urgent update (changes the button text) and returns back to the reconciler saying that *This time, I have 20ms to spare*, and this goes on till the computation of the whole tree is complete.   

This way of working, going down to do the computation(low priority updates) and coming back up to acknowledge changes (high priority updates) is what gives it a loopy curve appearance. This mechanism of weaving through is also called work-loop, and this is where *Fibre* gets it name from.   
![Work-loop]({{site.baseurl}}/images/how-react-works/work-loop.gif)

In the pursuit of getting a better understanding of what's happening, I also built the classic [space-shooter game]({{site.baseurl}}/projects). I'll keep updating my findings here.


* * * 

> *Some parts of this post are just an abridged version of [Lin Clark's Introduction to React Fibre at React Conf 2017](https://www.youtube.com/watch?v=ZCuYPiUIONs) & [Andrew Clark's Github repo](https://github.com/acdlite/react-fiber-architecture)   
This post was created solely based on my understanding of the work-flow, do suggest for improvements and changes.*