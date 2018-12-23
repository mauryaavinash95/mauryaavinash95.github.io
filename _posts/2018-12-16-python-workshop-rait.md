---
layout: post
title:  Python Workshop, RAIT
date:   2018-12-16 01:42:24 +0530
categories: Web Development
comments: true
tags: Python, Workshop, RAIT
---

*Intended for participants of python workshop, RAIT*

## Introduction				
PYTHON has grown into the most dominant, robust and elegant high-level programming language of the decade. It's been used widely in almost all aspects of engineering, ranging from networks, control system design, web-frameworks, machine learning, computer vision and what not.

Owing to your zeal to learn about various advancements of this field, I feel ebullient to welcome you all to this two-day hands-on python workshop.

## External Speakers			
1. [Adit Shinde (2017 Graduate) Currently working at JPMC](https://www.linkedin.com/in/aditshinde/)
2. [Avinash Maurya (2017 Graduate) Currently working at Peoplegrove Inc, previously Reliance Jio. ](https://www.linkedin.com/in/mauryaavinash)

## Volunteers who made it a success
1. [Vineet Kadam](https://www.linkedin.com/in/vinit-kadam-b26b39146/)
2. [Dhanesh Katre](https://www.linkedin.com/in/dkexception/)
3. [Tanay Joshi](https://www.linkedin.com/in/tanay-joshi-889903131/)
4. [Chetan Kuckian](https://www.linkedin.com/in/chetankuckian/)
5. [Viraj Khedekar](https://www.linkedin.com/in/viraj-khedekar-592182167/)

## Topics			
1. What is Python?
2. Why Python?
3. How do I get started?
4. Datatypes
5. Control flow
6. String, List & Dictionary Manipulations
7. Functions
8. Modules & packages
9. Python file operation
10. OOP 
11. Text Processing
12. Sample Project


## Installation			
#### Check if system has python installed			
Type `python3 --version` in your command-prompt/terminal, if system says `Python 3.x.x` then you've begun your journey to speak [Parseltongue](http://harrypotter.wikia.com/wiki/Parseltongue).		
#### Install python 		
Please visit [https://www.python.org/downloads/](https://www.python.org/downloads/) and goto previous step to check if it is installed properly.

## Repository for the source code
[https://github.com/aditshinde/python-training](https://github.com/aditshinde/python-training)

## Speaker slides
[Slides](https://docs.google.com/presentation/d/1aHcCz8ad03q9BL3x-W8gqSxJcVjBcz24LVEJa4Le2YI/edit?usp=sharing)

## Assignments
1. [Assignment Questions](https://docs.google.com/document/d/1PlMITiADLogd8NSyfkiYOQP85OsKBT6jsIVMspOV4OE/edit?usp=sharing)
2. [Assignment Solutions](https://docs.google.com/document/d/1RRIk8ln3E9hzLQrrWOzu5-or4jGVhh8dAgOyNDw6BdQ/edit?usp=sharing)
3. [Prettified questions & solutions]({{site.baseurl}}/images/python-workshop/questions-solutions.docx)

## Build it up in 1 week challenge
### Challenge 1: Hotel Management System (HMS)
#### Introduction
You are a wealthy owner of _Hotel Transylvania_, which contains 100 rooms, numbered from 1 to 100. 
There are plenty of guests who arrive & depart daily at your hotel. 
Each guest checks-in for the current day and checks-out at the end of the day.
#### Subtask 1
Build a menu-driven program to 
1. _(Create)_ Check-in guests by allocating the first available room number (lowest integer).
2. _(Delete)_ Check-out guests by clearing out the entry for the given room number.
3. _(Read)_ View list of empty rooms.
4. _(Read)_ View list of allocated rooms.
5. _(Update)_ Change name of a given guest: Should be an input box which asks for the new name of the guest for given room number (You can even show the previous name on the terminal for reference).

#### Subtask 2
The hotel contains some rooms for humans & others for monsters (in real world, you can consider for non-VIPs and VIPs, _I prefer to keep it a bit fancy though_), specifically the odd numbered rooms are reserved for humans & even ones are for monsters.		
1. Properly allocate a guest by asking if he/she is a 'H' (human) or 'M' (monster).			
2. Meal charges for H= $100 & M= $300, rent for both the rooms is $150 per day. Calculate the bill amount for each member at the end of the day (check-out)

#### Subtask 3
The hotel has expanded and increased its capacity to 1000 guests. 						
1. 	50 		Penthouses								
2. 	100 	Super Deluxes					
3.  200		Deluxes									
4. 	450 	Premiums								
5. 	200		Nirvana houses.				

Calculate fare for each room as `((6-(class of room)) * 100 + 250)`									

Calculate meal charges as `((6-(class of room))*20 + 5)`				

For instance: a penthouse's rental costs is (6-1)*100 + 250 = $750,						
similarly, nirvana house's costs (6-5)*100 + 250 = $350					

Your task is to calculate hotel fares for each individual accordingly.

#### Subtask 4
Make it all up in GUI (you can use tkinter or any other python GUI library).


#### Final
Send a working video of any and all subtask(s) to avinash [at] outlook [dot] com - I'll gladly feature it here on this page and invite you for my next hackathon tour. With a few tweaks it might be your first step towards open source contribution.



### Challenge 2: Commputer Lab Allocator (CLA)
#### Introduction
Your systems in labs are currently unenrolled. 			
Write a code to accept name & roll number of a student as soon as he/she logs in.

#### Subtask 1
1. Create a python file, which, on execution, opens a menu asking if user is student or admin. 
2. If the user is student, ask for name & roll no. (Later you can consider using a password based login)
3. _(Create)_ Write these values with timestamp to a CSV file (prefer writing in a hidden CSV file)
4. _(Read)_ Create an admin login (username: _admin_) who can login to your program and view all records
5. _(Delete)_ Admin can delete entry of a particular student
6. _(Update)_ Admin can change name/roll no. of a given student: Should ask if need to update name or roll no. (show previous name & roll no. for reference)

#### Subtask 2
1. Keep the CSV file password protected- Only your code should be able to access the file.
2. Only keep your executable (`.pyc`) file on the system, not `.py` file.

#### Subtask 3
Make it up in GUI.

#### Subtask 4 (Tough)
1. Use `MySQL` database for storing data instead of `csv` files: you'll need to use python libraries for accessing MySQL server.
2. Use a centralised `MySQL` database: Install `MySQL` only on one server and make network calls via internet/intranet to the database server.

#### Final
Send a working video of any and all subtask(s) to avinash [at] outlook [dot] com - I'll gladly feature it here on this page and invite you for my next hackathon tour. With a few tweaks, it might be your first step towards open source contribution.


### Challenge 3: Ping-Pong Game
#### All tasks

Build a ping-pong game-> Ball starts from top left, bounces on the bat, hits walls and returns back.
Don't let the ball hit the ground.
Use pygame instead of tkinter.

Write an article explaining how you built it, which libraries did you use and why did you use it.

#### Final
Send a working video of any and all subtask(s) to avinash [at] outlook [dot] com - I'll gladly feature it here on this page and invite you for my next hackathon tour. With a few tweaks, it might be your first step towards open source contribution.					


## College Feedback form
Please click [here](https://docs.google.com/forms/d/e/1FAIpQLSerBHaYJTzbR7IYo7MWAAb0EUhKAkSnSdRagzOdgqkaGToE0w/viewform?vc=0&c=0&w=1) and fill out the feedback.

## Some Memories...
![Inauguration]({{site.baseurl}}/images/python-workshop/inauguration.jpg)

![Happy All]({{site.baseurl}}/images/python-workshop/all-pic.jpg)

![Token of Appreciation]({{site.baseurl}}/images/python-workshop/token-of-appreciation.jpg)




# Hope to see you guys soon! Dasvidaniya!! 