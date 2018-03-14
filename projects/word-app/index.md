---
layout: page
comments: true
title: Word-App
---

## A simple dictionary with Google like interface.

To start clone the repo and run 
```bash
git clone git@github.com:mauryaavinash95/app.word-app.git
cd app.word-app
npm install
npm start

git clone git@github.com:mauryaavinash95/api.word-app.git
cd app.word-api
npm install
npm start
``` 
To visit the live version of the app, visit [https://app-word-app.herokuapp.com](https://app-word-app.herokuapp.com).      
   

### Motivation
The inital idea was to create something simple and mix up all the things learned so far (redis, elastic-search, aws deployments and react) with it. That is when one of my colleague, [Deviyani](https://www.linkedin.com/in/deviyani-gaur/) came up with this idea of Word-App. It seemed fairly conceivable and incorporate all those things in one. So I ended up making a dictionary application and then later converted it to a Progressive Web App (PWA). 

### The making
#### BACKEND
This area is relatively plain and blunt. Just some Restful APIs which handle actions like `login`, `signup`, `find-word`, `save-to-favorites`, `get-favorites`, `get-history`, `suggest-word` and `newsubscription`.        
The backend consists of Express as server, MongoDB as datastore and Redis as datacache and Elastic Search (ES) as a suggestion toolkit.      
Diagramatically,        
![Word-App-Backend-Architecture]({{ site.baseurl }}/images/word-app/word-app-1.png)         

The flow is something as under:
1. User Sign-up: `/post signup` express route takes the required parameters and stores it in `users` collection of datastore. Alongwith username and password, randomly genereated strings `userId` and `token` field are stored alongwith (Random Strings are generated using [short-id](https://www.npmjs.com/package/shortid).        
2. Login: `/post login` route takes username and password as params and sends back the `userId` and `token` which are further used for validation of all requests.
3. Find word: `/post find` takes `userId`, `token` and `word`(word to be found) and searches for it first in the datacache, if available, it returns the response from there or falls back to datastore which finally falls back to [Oxford dictionary APIs](https://developer.oxforddictionaries.com/documentation). 
It also maps the word to users' history collection (For Recent words list).          
Also, as more and more times a given word is searched for, its weight in ES server gets incremented.     
4. Save word (Mark as Favorite): `/post save`, this takes params similar to `/post find` and check if the given word exists, and then marks it as favorite for the given user.
5. History (Recent): `/post history`, this route takes `userId` and `token` as params and returns the list of all words visited by the user in chronological order, alongwith favorite status.      


#### FRONTEND
As the subtitle of this post suggests it's *A simple dictionary with Google like interface*. Getting a Google like interface is relatively simple with packages like [material-ui](http://www.material-ui.com/) and [material-design-lite](https://getmdl.io/), so why reinvent the wheel? I just went ahead and used material-ui for this project.         
* Now the API calls were just a couple of `fetch` requests, but their orientation and sizing still took some efforts.       
* Breaking it down, I had only 4 distinct routes: `/`(login), `/signup`, `/home`, `/recent` and `/favorites`.       
* Home has the (autocompleting) search bar which fires a get request after 3 or more letters to ES server, which responds with a list of proabable words, depending on their weights.           
![Word-App-Home]({{ site.baseurl }}/images/word-app/word-app-home.png)          
* Recent and Favorites have a list of toggle-able word-cards which make a `fetch` request on-click and present the meaning of the word on the expanded card.  
![Word-App-Home]({{ site.baseurl }}/images/word-app/word-app-favorites.png) 

### Deployment
The initial deployment was done on [Heroku](http://heroku.com) for both frontend and backend, but then it too much load time (yes, it because of the dynos sleeping), so I shifted the backend to AWS and am thinking to do the same for frontend and provide a 301 of herokuapp url to the new route.   
So I migrated the backend to AWS EC2, the free tier ofcourse, one because it's free and two because I as a developer had much more flexibility in terms of operations for my other projects as well.     
Coming to the database part, I used the free datastore from [Mlabs](https://mlab.com/) for MongoDB, [Redislab](https://redislabs.com/) for Redis and [Bonsai](https://bonsai.io) for ES.     


### Conversion to PWA
* This topic (PWA) has gained quite some heat in recent times, so wanted to try this out as well. There were loads of awesome tips, tricks and tuts at [Google Codelabs](https://codelabs.developers.google.com/) and [Maximilian Schwarzmüller's](https://mschwarzmueller.com/) [course on udemy](https://www.udemy.com/progressive-web-app-pwa-the-complete-guide/), so I gave it a go in this project.       
* PWA is a bit demanding in terms of security though, but it rightly has its reason for it. This required me forcing HTTPS for frontend. On heroku that isn't very complicated right? *You just add a `"https_only": true` to your static.json*.  
* Now since frontend was working on HTTPS, it wasn't able to make calls to HTTP backend, again security reasons. So this time, I had to obtain a SSL certificate for PWA to work. That's when I first learned about [Let's Encrypt](https://letsencrypt.org/). Through some trails-and-errors I finally came to know that it's not going to work on `public ip address`, `elastic ip address` or `instance url` of my EC2 or some free domain with redirects or iFrame embeds, so I had to buy a domain, change its DNS records and then finally was able to achieve a valid SSL.     
* Again, to save some later time and effort, I routed the backend traffic through [Nginx](https://www.nginx.com/), forced HTTPS on that as well and added some location proxies for various projects. And I came to a realisation that Nginx is simply :heart_eyes:
* Backend was a bit tedious as compared to frontend (obviously :smile:), but it was fun doing that as well.
> More on this coming up in a blog-post.

### Conclusion
There were a couple of gotcha's that came every now and then while the whole journey but then that wavy ride was fun working with.



