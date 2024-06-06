# Service Workers

A Service Worker is a JS file running in its own thread that will act as a middleware offering a local installed web server or web proxy for the PWA, including resources and API calls.

Service Worker: 
- It runs like client side server.
- It can serve files, can answer HTTP requests like a server.
- Runs client-side in browser's engine.
- HTTPS required (the exception is 'localhost').
- Installed by a web page.
- Own thread and lifecycle.
- Acts as a network proxy or local web server in the name of the real server. So the PWA will never know whether the response is coming from a server or a Service Worker.
- Abilities to run in the background.
- No need for user's permission.
- We can write service worker code in one or multiple JS files.
- When we are in the Service Worker, after a few seconds of network inactivity, the thread will be terminated. This is why the Service Worker is not the right place for all kinds of content. E.g. if we want to communicate with a server, 


## Scope

Every Service Worker has a Scope.  
Scope: An origin (host and port) and a path.  
If service workers are in different folders, we can have multiple service workers in our code.

### Service Worker and Scopes

- It manages all pages within browser and within installed app from scope. Every HTML and URL will be managed by Service Worker
- It's installed by any page in the scope.
- After installed, it can serve all files requested from the scope.
- Only one service worker is allowed per scope.
- Webkit adds partition management.
- Usually we install service worker immediately. But if needed, we can install it later.
- We don't need to install PWA to get a Service Worker.

chrome://serviceworker-internals/ - we can use this link to debug all the service workers that are currently installed.


## Register Service Worker

1. ```<script src="sw-register.js"></script>```
2. ```serviceworker.js``
3. 
```js
// main thread
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register("serviceworker.js");
}
```

## Caching and Serving PWA Resources

### Caching resources

- Service worker has a local cache - Cache Storage API (also available outside the service worker).
- We can cache all or some resources.
- JavaScript promise.
- Prefetch on installation. When we instll the service worker, we we download the assets we will need later.
- Cache on request. Instead of precaching we can cache a request.
- App Shell pattern. This means that we download at least all the necessary files that are needed to render app shell (everything we see in the user interface).


### Serving resources

- The service worker will respond for every request the PWA make.
  - It can serve from the cache
  - It can forward the request to the network
  - It can synthesize a response (create a response on the fly).
  - Any mixed algorithm is possible

When we write Service Worker, we use ```self``` instead of ```this``` because using ```this```might cause some problems.

We don't need to cache: 
- app icon, because it is installed by the browser. 
- web manifest - it happens around the PWA, not in the inside of PWA.

We can cache resources from other domains, but we can use those resources for our scope, our PWA, not for the others.

Trick: Shift + refresh🔄️ will bypass the web worker.

[Workbox](https://developer.chrome.com/docs/workbox/) a library for service worker.


### Cache Serving Strategies

- Cache first.
- Network first.
- Stale while revalidate.


When we update the code, we can make some minimal change, like a comment, in the service worker file, and reload the page twice, a change will be reflected.

Another way is to use Network first method. But there is a problem with updating the assets.

The third solution is Stale While Revalidate:   
Goes to the cache and looks if cache is there ->  
It will return cached response / or will return the ferch.

In this case it always goes to the network. It also checks the cache, but also goes to the network, gets the resource from the network (e.g. PNG) and updates the cache.  If the resource is in the cache, it will give thre resource from the cache. Every time updates the cache, so that the next time the user is requesting that file, it will be an updated version.


## Updating Resources

- Files are saved in the client
- Updating files in the server won't trigger any automatic change in the client
- So We need to define and code an update algorithm
- It will need a process within your build system for hashing or versioning files

So if we have a build process, such as we are doing a react application, sometimes we hash resources, or we version resources, we can create Manifest file, separated from our Web Manifest that can define all the assets with a version number. and then the Service Worker can see and compare the assets and update all the assets that are old in the cache.

Developer is in full control of how to cache and serve the resources of the PWA, and how to manage API calls.

Updating the app doesn't require full reinstallation; we can just replace the updated files silently or with a user's notification


### Lifecycle

When the PWA is installed We don't have a Service worker. Then service worker is: 
1. Parsed.
2. Installing.
3. Waiting.
4. Activating.
5. Activated. - Idle.
- It is waiting for an event: fech, push or a message to run.
6. Running - After an event, service worker starts running.
- After some time of network inactivity (in Chromium 40 seconds. also, during this waiting time, browser can kill the process if it thinks there is something wrong going on, e.g. to the CPU) goes bacj to Idle.
- After an event, it goes back to running.


Every time Service Worker appears, it might not be the same instance. So we don't want to use global variables in the Service Worker's scope, because they might be gone.




## Update a SW
 
- Change the file on the server
- Browser will check on new instances
- Work with HTTP Cache headers (Max 24 hours)
- registration.update()


If we have a bug, we just need to upload a new version.  
The browser has an update algorithm. When the browser sees that there is a new version, it will prepare that new version for the next user. So there will always be one version of the service worker that will be old, but then, on the next load,  in a minute, after the old one was killed, we will see the new service worker instance. 


chrome://inspect/#devices we can use this to run our local code in the laptop, e.g. in our phone.  
It is going to compile our app in Google Servers. It is called Web APK Mint server.