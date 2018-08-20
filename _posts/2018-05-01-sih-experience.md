---
layout: post
title:  My Smart India Hackathon 2018 Experience
date:   2018-05-01 21:42:24 +0530
categories: Hackathon
comments: true
tags: Digital India, SIH 2018, World's Largest Hackathon, Winning SIH 2018
---
[Smart India Hackathon](https://innovate.mygov.in/sih2018/), or SIH as it is generally called, is World's Largest Hackathon, in which more than 10K teams participate to solve various social, political, medical, scientific and technological issues of the Indian masses. Started off as a scheme of Digital India Initiative by Honourable Prime Minister Shri Narendra Modi in 2017, it embarks the journey of young citizens of India towards a crowdsourced & entrepreneurial development.

### How all of this started?
The tradition of participation in SIH started off in 2017, where Team Graminunnati (Consisting of [Avinash Maurya](https://www.linkedin.com/in/mauryaavinash/), [Sarthak Landge](https://www.linkedin.com/in/sarthaklangde/), [Abhi Savaliya](https://www.linkedin.com/in/abhisavaliya/), [Aditi Sakhre](https://www.linkedin.com/in/aditisakhare/), [Devesh Naik](https://www.linkedin.com/in/devesh-naik-2102/), [Harshit Virkar](https://www.linkedin.com/in/harshitvirkar/), [Dhanashree Bhosale](https://www.linkedin.com/in/dhanashri-bhosale-93680778/) and [Yashraj Rai](https://www.linkedin.com/in/yashraj-rai-19527a25/)) was the first team from our institute to be selected for participation. And after bagging the prize then, our Director, [Professor Ramesh Vasappanavara](https://www.linkedin.com/in/ramesh-vasappanavara-71a05939/) was confident that our student community could offer a lot to the society and hence encouraged & funded much more teams this year, one of which was Team Big-Oh. Special thanks to him for always being the influencer and a guiding light for all of us.

### Team Big-Oh: Some Introduction first
The team consisted of 6 participants and 2 mentors, and as it turns out, I was one of the mentors this time. The team hailed from RAIT, Nerul, Navi Mumbai.     
[Akash Jaswal](https://www.linkedin.com/in/aakash-jaswal-92866a126/), the team lead- *the zealot*, was the source of energy and innovation for everyone; [Rammit Kovvalpurail](https://www.linkedin.com/in/ramit-kovvalpurail-621298131/), *the scrupulous & phlegmatic one* was the one who steered development of 3 android apps and even built 2 of them single handedly. [Harshal Desale](https://www.linkedin.com/in/harshal-desale-460522135/), *the perspicacious one* and [Pratik Salunkhe](https://www.linkedin.com/in/pratik-salunkhe/), the *the astute & audacious one* managed to learn and implement technologies like React, NodeJS and Firebase within a short timespan of 36 hours. [Shrey Dhingara](https://www.linkedin.com/in/shrey-dhingra-12754419/), *the versatile one*, was the one who juggled in between different technologies and interfaces and glued all things together. And [Ayushi Agarwal](https://www.linkedin.com/in/ayushi-agrawal-a5b481152/), the youngest and *the vibrant one* was the pioneer for analytics and dev-ops. The backbone of this all, [Yashraj Rai](https://www.linkedin.com/in/yashraj-rai-19527a25/), *the awesome-est one*, was the idea hamster, meticulously taking into consideration the edge cases & repurcussions of all the technical & administrative requirements. And lastly, I was assigned with the duty of mapping things from real world to technical one and making the system work offline.          

### The prep work
Our preparations started approximately 2 weeks prior to the hackathon day. On weekends, the Big-Oh team would get-together at college and brainstorm ideas. The initial days saw an upsurge of ideas from each and every individual and in the subsequent meetings, we were carving the best of those ideas to build up a next generation solution for the Indian masses. Every feature underwent an elaborate discussion with mind-maps, use-case diagrams, wire-frames, architecture, database schema, real-time processing and our Unique Selling Point. At the end of it, everyone had a cogent view of what we wanted to build.    
Since pre-cooked codes were allowed, the team started building up the preliminary interfaces and looking into the implementation aspect of things, shuffling features up and down to facilitate efficient delivery of the overall product. We already felt fervently cheerful knowing how exciting the 36 hour journey is going to be.  
The sheer diligence & alacrity of the team left many participants, mentors, coordinators and faculties awestruck. 

### Our problem statement first
The problem statement was titled *"Ambulance tracking system for 108 services"* wherein we were supposed to deliver a software based solution for efficient addressal of victims of Accident, Emergency and Fire. The task was to implement something which facilitates the victims or passer-bys to immidiately invoke medical, police or fire services without involving manual interactions with these services. 

### What we did in those 36 hours?
Having our wireframes and roadmap ready in front of us, our initial development process was at a great pace. Everyone had their tasks and sub-tasks divided and were peforming at their optimal speed. However, after the first & second round (after 6-8 hours) of evaluation, we realised that we ought to take more practical and real-life situations into scope of our development. Thus, we had to further accelerate our development timelines and be done with our basic prototype in the first 12-14 hours. Everyone was so meticulously involved that they sparingly had their meals in order to allocate that time for development purposes.             
After being done with our very primitive prototype of 3 mobile apps and 1 web app in 14.5 hours, we spent approximately 1.5 hours doing sanity & integration checks and VOILA!!, it all stiched up beautifully, everything was working perfectly, as expected.      
After this came a moment wherein we again had to stop and brain-storm the real-life and edge cases pointed out by the jury in initial phases of evaluation. It took nearly 3 hours to come up with our USP and address the pragmatic issues. It was indeed worth the time and effort because it was going to be a deciding factor whilst competing with other 200 teams at the venue. The major challenges were - offline functionality and precise geolocation of the victims, both of which were conundrums for us.  
#### About offline functionality:
How do we take location coordinates of the victim offline and send off alerts and tracking to ambulance driver, police and relatives?   
Here's when it struck me that in a similar attempt  researchers at IIT KGP transmitted data offline to their APIs and established offline connectivity. So we started sending off data packets over SMS and integrated webhooks to our APIs to achieve offline functionality for our apps.

#### How to improve geolocation precision?
The rural terrains of India don't even have unpaved roads for most of the places. And in overcrowded metropolitian cities, it again becomes nearly impossible to exactly trace an apartment and reach there.    
To solve these mysteries, Akash and Pratik came up with [plus.codes](https://plus.codes/) to provide much better geolocating systems. At the time of our hackathon, plus.codes was just 16 days old, but already looked very promising and came in handy because:
* It worked offline.
* It assigned addresses even for places where there are no roads. 
* It was ideally built for emergency services.

### What did we achieve?
At the end of hackathon, not only did we achieve a fully working prototype with a sense of contribution towards the society but also the grand prize of 100K INR. To further appreciate our efforts, chairman of the host institution, Techno India Group, Dr.Goutam Roy Chowdhury even awarded 10M INR startup venture fund.       
The Ministry and Indian Govt. lauded the meticulous intricate details, technological front-foot & robustness of the system and decided the take the system ahead to implement for the Indian masses.        
It was truly a phenomenal success for all of us afterall, with 2 weeks of dedicated preparation, 40+ hours of non-stop coding and ebullient team-efforts we got home the trophy.

### Our special moments together
![Team Pic]({{site.baseurl}}/images/sih-2018/sih-2018.png)

### Media:
Twitter: [BigOh, winner team, shared their winning story & about their journey of ideas to implementations.](https://twitter.com/GeekonixEdge/status/980626539787075584?s=17)       

Times Of India: [Hackathon: Coders bag Rs 1 crore](https://m-timesofindia-com.cdn.ampproject.org/v/s/m.timesofindia.com/city/kolkata/hackathon-coders-bag-rs-1-crore/amp_articleshow/63673673.cms?usqp=mq331AQECAE4AQ%3D%3D&amp_js_v=0.1#referrer=https://www.google.com&amp_tf=From%20%251%24s)