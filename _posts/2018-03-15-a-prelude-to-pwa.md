---
layout: post
title:  A prelude to Progressive Web Apps(PWA)
date:   2018-01-30 02:00:24 +0530 
categories: React
comments: true
tags: React, Frontend, Service-Worker
---
We all keep hearing the transitioning of everything from a web-based world to a mobile first world. This is making the web-technology groups engender more and more features, Progressive Web Apps (PWA) is one such step along that path. The core idea behind PWA is to progressively & gradually enhance your web-apps to make and give a native like look & feel.    
This brings us to the question:
> *What is it that web-apps lacked earlier as compared to native apps?*           
> Offline functionality, Use of basic device features like camera & location and Push Notifications.    

## How it came to me?
I was introduced to some of the concepts of PWA whilst browsing on [Google Codelabs](https://codelabs.developers.google.com/), it seemed really fun to right-away start working with it, so I started taking some time off my routine and got indulged in learning that.

## First, some gentle intro...
PWAs function like any regular website, but if the clients' browser has support for it, they can use the features like installation on Home Screen, caching and notification. There are two main files which make these apps PWA, namely *manifest.json* (strictly should be named as manifest only) and *service-worker.js*.       

## Understanding Manifest *(the easy one)*:
This file is reponsible for apprising the browser the various properties of our application. This file is mainly used while installing the app, the browser reads these properties and sets a color, theme & icon for the application accordingly. These properties are described as follows:
```json
{
  "name": "Word App",
  "short_name": "Word App",
  "icons": [
    {
      "src": "/icons/app-logo-144x144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "/icons/app-logo-192x192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/icons/app-logo-256x256.png",
      "type": "image/png",
      "sizes": "256x256"
    },
    {
      "src": "/icons/app-logo-384x384.png",
      "type": "image/png",
      "sizes": "384x384"
    },
    {
      "src": "/icons/app-logo-512x512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/",
  "scope": ".",
  "display": "standalone",
  "orientation": "portrait-primary",
  "background_color": "#fff",
  "theme_color": "#3f51b5",
  "description": "A simple Word App, implemented using PWA.",
  "dir": "ltr",
  "lang": "en-US"
}
```
Nothing fancy here, just 
* A couple of `icons` for varying device resolutions 
* `start_url` defines the landing page of the application (it can also be something like `/homepage.html`).
* `scope` defines the relative URL of the paths of the web pages which can be viewed, if user navigates to a path not included in the scope of manifest, it behaves as a normal web-page, opening up in the browser not the app anymore. Eg. if `scope: "/home/"` and user tries to visit `www.example.com/channels`, it would redirect the user to a normal web page.
* `display` enumerates [`fullscreen`, `standalone`, `minimal-ui`, `browser`]. More on this [here](https://developer.mozilla.org/en-US/docs/Web/Manifest#display)
* orientation can be one of the following [`any`, `natural`, `landscape`, `landscape-primary`, `landscape-secondary`, `portrait`, `portrait-primary`,`portrait-secondary`]. It defines the default orientation for all the web application's top level browsing contexts.
* `background_color` is primarily the color of splash screen of the application.
* `theme_color` is used for header area of app control screen.      

## Understanding Service-Workers (*this is the one with powers*) 
Now that the installation process is sorted out by *manifest.json*, there are things like caching and push notification remaining for the service-worker to do. 

### What exactly is service-worker?
Service Worker is a regular javascript file which gets fetched on loading the website, but browsers treat this `.js` file a bit differently. Browsers install this file and allocate an entirely different thread for the service-worker pertaining to a given domain URL. This thread is forever active in the background even when the tab is closed or browser is entirely shut. So this is a process which is always active, and hence its a great place for implementing listen events of push notifications or network-proxy or behind the scenes caching or syncing up some data as soon as the user gets online.        

### Great, but how to do all of this?
With all the advancements being done in the browsers, there are a lot of tools and APIs which are being exposed to the end-developer. Some of them being `navigator`, `caches`, `indexedDB`, `webSQL`, `localstorage`, `lighthouse`, etc. so there isn't much of third-party resources involved.       

#### Step 1: Check if the client-browser supports service-worker:
Generally this code is written in the index file. (We can choose any other file to have this code, it checks if the given client-browser has any object named `serviceWorker` in its `navigator`'s context).        
If the browser supports serviceWorker, it registers a serviceWorker present with the file named `/sw.js`.       
The scope of serviceWorker is restricted to it's parents' route, so if a serviceWorker is present in */public/myapp/*, it cannot access events from */public/another-route/*, thus serviceWorkers are generally kept in the public scope (*/public*).

```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
    .register('/sw.js')
    .then(function () {
      console.log('Service worker registered!');
    })
    .catch(function (err) {
      console.log(err);
    });
}
```

Directory structure:
```
├── 404.html
├── favicon.ico
├── help
│   └── index.html
├── index.html
├── manifest.json
├── offline.html
├── src
│   ├── css
│   ├── images
│   └── js
└── sw.js
```

#### Step 2: Installing and Activating serviceWorker.
The serviceWorker goes through the following lifecycle phases:
1. **Install**: This event gets triggered when the domain URL is first hit or there is a change of one or more bytes in the serviceWorker file. In case of revisits, the serviceWorker file is not cached and hence everytime this file is fetched from the server. If this is same as the one installed, no action is taken, else install the new serviceWorker.             
2. **Waiting**: This happens if there is a previous serviceWorker active and there is a window open for a given URL, the new service worker enters into *waiting* state. The reason for this behaviour is because if the currently open window uses the previous serviceWorker, there is a probability that the new serviceWorker might break its functioning.            
![Service worker waiting]({{site.baseurl}}/images/word-app/word-app-service-worker-waiting.png)
3. **Activate**: This fires once the old service worker is gone, and your new service worker is able to control clients. This is the ideal time to do stuff that you couldn't do while the old worker was still in use, such as migrating databases and clearing caches.          
![Service worker activated]({{site.baseurl}}/images/word-app/word-app-service-worker-active.png)            


#### Step 3: Caching assets - Static Caching:
The core idea is to cache the static assets like *css*, *js* and *images*. Generally the app-shell model is used to cache as soon as the serviceWorker is activated.    ![App Shell Model]({{site.baseurl}}/images/word-app/word-app-appshell.png)      
Coming to the code:   
```javascript   
// sw.js file   
const CACHE_VERSION = "v1.1";
const CACHE_STATIC_NAME = "static-" + CACHE_VERSION;
const STATIC_FILES = [
    "/",
    "/static/css/main.css",
    "/static/css/main.css.map",
    "/statc/js/main.js",
    "/statc/js/main.js.map",
    "/static/js/localforage.js",
    "/favicon.ico",
]

self.addEventListener('activate', function (event) {
    event.waitUntil(
        caches.open(CACHE_STATIC_NAME)              // Opens the cache named static-v1.1
            .then((cache) => {                      // Returns the cache object to perform operation on
                console.log("[Service Worker] Precaching App Shell: " + CACHE_VERSION);
                cache.addAll(STATIC_FILES);         // Takes Array of files, makes a request for them and saves them in cache
            })
    )
});
```         

#### Step 3: Making it a network proxy  
Since serviceWorker always stay active, it can listen to all the network calls a particular URL makes, and respond with resources from cache whenever required.
```javascript
//sw.js file
const CACHE_DYNAMIC_NAME = "dynamic-" + CACHE_VERSION;
self.addEventListener('fetch', function (event) {
    if (isInArray(event.request.url, STATIC_FILES)) {           //Checks if the requested resource is present in list of STATIC assets, it returns the response from cache.
        event.respondWith(
            caches.match(event.request)
        )
    } else if (event.request.url.indexOf("/find") > -1) {       // More on this in the next step, for now just consider it another conditional clause.
        event.respondWith(find(event));
    } else if (event.request.url.indexOf('/sw.js') > -1) {      // Go ahead and make a fetch request to the server everytime for sw.js file.
        event.respondWith(fetch(event.request));
    } else {                                                    // In all other cases, check if resource is available in cache, 
                                                                // if present, return response from cache,      
                                                                // else make a network request, save the response and send it to the requested asset as well.
        event.respondWith(
            caches.match(event.request)
                .then((response) => {
                    if (response) {
                        return response;
                    } else {
                        return fetch(event.request)
                            .then((res) => {
                                return caches.open(CACHE_DYNAMIC_NAME)
                                    .then((cache) => {
                                        cache.put(event.request.url, res.clone());
                                        return res;
                                    })
                            })
                            .catch((err) => {                   // In case the fetch request fails and the asset isn't cached either, we show our custom 404 page.
                                console.log("In error: ", err);
                                return caches.open(CACHE_STATIC_NAME)
                                    .then((cache) => {
                                        return cache.match("/404.html");
                                    })
                            })
                    }
                })
        );
    }
});
```

#### Step 4: What about my API responses or JSON data?
Caches are generally used for text, blobs or static files, they aren't ideally suited to store key-value pairs as such, we need a database to manage all that.   
This is where *indexedDB*, *webSQL* and *localstorage* come into picture. Different browsers have different support for each of them, so there may be some browsers which do not support *indexedDB* or *webSQL*.    
Rather than checking everytime which DB is present, [*localforage*](https://github.com/localForage/localForage) helps by created a wrapper it and checks for them in the following order: *indexedDB*, *webSQL*, *localstorage*.

```javascript
function find(event) {
    let c = event.request.clone();                      // Clone the event.
    return new Promise((resolve, reject) => {
        c.json().then((requestBody) => {                // Convert the stream to JSON
            let wordToFind = requestBody.word;          // Find the word user is trying to search for
            localforage.getItem(wordToFind)             // Check in localforage if the word exists
                .then((result) => {
                    if (result) {
                        resolve(new Response(JSON.stringify(result)));  // If localforage contains a response for the given word then send that respond
                    } else {
                        fetch(event.request)                            // Else make a fetch request
                            .then(response => response.json())              
                            .then((responseJson) => {               
                                resolve(new Response(JSON.stringify(responseJson)));        // Send the response back
                                localforage.setItem(wordToFind, responseJson);              // Save the word in localforage for future responses
                            })
                    }
                })
        })
    })
}

```

## Conclusion
This was a very minimalistic and naive use-case of PWAs. Learnt a lot of new things while building up the [word-app](http://app-word-app.herokuapp.com). Read its making [here in project post]({{site.baseurl}}/images/projects/word-app/).  
![Word-App]({{ site.baseurl }}/images/word-app/word-app.gif) 

> More on push notifications coming soon. Stay tuned...