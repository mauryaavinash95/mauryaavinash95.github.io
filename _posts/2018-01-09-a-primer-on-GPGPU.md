---
layout: post
title:  A Primer on GPGPU
date:   2018-01-10 21:42:24 +0530
categories: Parallel Programming
comments: true
tags: Parallel-Programming,  NVidia's-CUDA, GPGPU
---
When we started building [this project]({{site.baseurl/project/gpu-computing-using-cuda/}}), we had to search around for a lot of resources. Though there are some really good presentations of [NVidia's GPU Conf](http://www.nvidia.com/docs/IO/116711/sc11-cuda-c-basics.pdf) and [this course](https://classroom.udacity.com/courses/cs344), its still seems somewhat complicated. I'll try to explain it as simply as possible.
> Special-mention: [Somesh Musunuri](https://www.linkedin.com/in/somesh-musunuri-10639811a/), [Naveen Peddyreddy](https://www.linkedin.com/in/naveen-peddyreddy-a05344142/), [Ameya Mhatre](https://www.linkedin.com/in/ameya-mhatre-46a660141/)

## What is CUDA
CUDA, earlier known as Compute Unified Device Architecture, is a framework developed by NVidia to perform some general purpose calculations on the Graphics Processing Unit (GPU). I know it sounds a bit *boring* and *jargon-ish* by definition, but eventually it'll start making more sense.     

### What is GPU
You able to see this nice text on your screen? Or some funny cat video on Instagram? Well that's a thanks to your GPU. This GPU is primarily responsible for showing or rendering the graphics on your screen. Those pixels you see constantly change in a video are because the GPU processes those images/fonts and changes each pixel on your screen for this great look.    
The reason pro-gamers prefer high-end GPU devices is so in their gameplay, those pixels can be updated at a greater frequency, thus giving them a smooth gaming experience. If the same game was to be played on a not-so-great device, it would lead to a very jarring motion, ruining up the whole experience.        

*Here's one of those classic ones*:  
![Graphic card]({{site.baseurl}}/images/introduction-to-cuda/graphic-card.png)


### The story so far

Till now, most of the computational tasks (like running a program, computing values, running simulations, running mathematical equations) are performed on the processors of Central Processing Unit(CPU) of our computers. Now, we want to run things more faster, not only that also we want to run multiple apps simultaneously, how do we go about that?         

**Step-1**  
To make systems more progressive, the frequency of processors was increased to improve their throughput (*Jargon, eh? Frequency corresponds to the number of ticks the processor makes in unit time. So more ticks means more tasks could be executed in unit time, thus leading to greater number of tasks getting completed per unit time aka higher throughput*). But then these semiconductors have their limitation, no matter how we dope them, after a certain point it isn't technically feasible to raise their frequency further.   

**Step-2**      
Now it was time to try something other than frequency improvement. Now people started increasing the number of processors in a system. So now, there are multiple workers to handle all the tasks and again we get a high yeild/throughput. But, again this had a limitation, with more processors, we had to deal with their shared resources (*What if both processors want to use the same device, say data-bus at a given instant*), intercommunication (*If the results of one processor are dependent on another one, how will they communicate and share that result?, will the processor wait till completion of another one to get that partial result?*) and a lot of other factors. So this multi-core architecture couldn't be amplified to a very great extent.

**Step-3**    
A lot of advancements in both the above scenarios is happening gradually, but as an alternative to meet our rigorous computing needs methods like GPU-computing, Grid-computing etc are being considered right now.     


## GPGPU
GPGPU is the abbreviation of General Purpose computing on Graphics Processing Unit. This is one of the alternatives being heavily considered for meeting our computing needs. As the name suggests, in this technique, we aim to do General Purpose computations, like mathematical calculations, running simulations, etc on GPU.      
So now the plain old GPU is not just sitting there for beautifying your screen, it got some real computational work to do, say something like adding two vectors.       
And this is where CUDA jumps in, it let's you instruct the GPU to do these calculations for you.       

#### Why to think about performing these computations on GPU?
Before we proceed any further, I'd like to explain the terms *latency* and *throughtput*    

**Latency**     
Simply put, this term refers to how much time it takes to completely finish a task i.e. how fast can the process be completed.

**Throughput**  
Throughput is the yeild of the processor i.e. how many number of tasks can it finish in a given amount of time.

I'd like to put it something like this: 
Consider we have a superfast car and a public transportation bus, having seating capacities of 2 people and 50 people respectively.       
Consider a scenario such that, the bus travels at some 50 km/hr and the car travels at some 200 km/hr. There are 100 people who need to be transported from given city *A* to *B* whose distance is some 150 kms.   

Now let's break down it down and do some math:
* The car would have to take `100/2` = 50 round trips to transport them all.    
* The bus would have to take `100/50` = 2 round trips to transport them all.
* Each round trip time for car = `2*150/200` = `2*0.75` = 1.5 hours. 
* Each round trip time for bus = `2*150/50` = `2*3` = 6 hours.

*Pheww... a lot of computations already... Don't worry, it gets simpler down here.*     

From the above data,    
We can see that the car took only 1.5 hours as compared to the bus which took 6 hours for every round trip, i.e. car can process faster as compared to car, so we say **"the car has a lower latency as compared to the bus"**.     

But, the bus was able to do more work in a given unit of time, because in 6 hours it transported 50 people, whereas the car would complete only 4 trips in 6 hours and thus transport only (4*2) 8 people in that time i.e. **"the bus has a higher throughput as compared to the car"**        

Final verdict, the bus would transport these 100 people in 12 hours and the car would take 75 hours for the same. For brevity, consider latency as how fast one person can be transported and throughput as amount of people transported per unit time. So greater latency doesn't necessarily mean higher throughput, though sometimes they are correlated.        

A very synonymous case is with GPU(*the bus*) and CPU(*the car*), CPU is built with the focus of minimum latency, to increase the execution time of any process, whereas GPU is focused for maximum throughput.     
Conventionally, it(GPU) needed to process a lot of pixels simltaneously, it did each process slow as compared to CPU, but it did a lot of them at once and thus produced a higher yeild.      

Moving on,          

The seats of the vehicle can be mapped to the cores of our device (CPU / GPU) and the speed can be compared to frequency of these cores.        

![Bus-vs-Car]({{site.baseurl}}/images/introduction-to-cuda/bus-vs-car.png)          

These ALUs (*Green Units*) in the above picture are responsible for processing and are also known as cores.

By these simple logics, we have eastablished substantial reasons to start using GPU for our computing needs, GPU seems to be way faster the CPU, then why have we not yet replaced CPU?             




> Hope this gave you some insight and interest in GPGPU.        
> Wish to read on?              
> Stay tuned, more coming soon...       










<!-- > Just for explaining this mechanism, let's consider that we'd be doing a very computationally intensive calculation, one with order of magnitude of about 10^6, techincally speaking 1e6 iterations.    -->



