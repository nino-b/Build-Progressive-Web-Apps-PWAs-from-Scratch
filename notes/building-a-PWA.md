# Building a PWA


### What is a PWA

PWA is the current DNA of the web.  
It is an app built with web technologies.  
It is used to create an app like experience using just web.  
It can work online and offline.  
It is installable.  
Icon will be standalone experience - from user's experience, it will be rendered outside the browser.


PWA is ideal for all those apps that consume web content or web services.

We can turn existing website into PWA.



## Compatibility

PWA is a website, so  
Every web browser is compatible with PWA.


## When is PWA installable

### Mobile support

On android we get offline support and installability from most of the browsers: 
- Chrome, Firefox, Opera, MS Edge, Samsung internet browsers.

On IOS:
- Only from Safari.



### Desktop

On desktop only Chrome and MS Edge are PWA compatible.

- Chromebook.
- Windows 7, 8, 10, 11.
- MacOS.
- Linux.

### Mini mode

PWAs have mini mode - smaller window version than a browser's.



### PWA support

Offline support within a browser: 93%.  
App experience with icon installation: 86%.


### No Support or plans from

- On Android: Facebook Webview (when we click a link in e.g. messanger, the link is rendered in an inner browser, not in a real browser).
- On iOS iPadOS: Apple watch, Google (on iOS when we use Google, the rendering engine is still Webkit, not Chromium), Facebook Webview, Firefox, Opera, MS Edge.
- On Windows / Linux: Firefox (supports on mobile), Opera (supports on mobile).
- On MacOS: Safari, Firefox, Opera.  


- Smart TVs.
- Smartwatches.
- Consoles.
- XR Headsets.

Microsoft Store for Xbox and Hololens support PWA / Oculus future plans.




# PWA Components

Three Main Components of a PWA:

1. Web App.
2. Web App Manifest.
3. Service Worker.


We can check whether a website is PWA or not in Google Devtools in LightHouse.

PWA won't work over the file protocol. Everything that we need to pass the PWA criteria, will work only over a web server.


## Web App Manifest

Web App Manifest if the first step of creating a PWA.

Web App Manifest is a JSON file that we need to create apart from the HTML. It is like a Heart of PWA.  

PWA have one Manifest. If there are two Manifests, there are two PWAs.


The JSON file is usually named 'manifest.webmanifest'. It should be written in json file format, so it can also be called 'manifest.json', but it is not common. Standard says 'manifest.webmanifest'.  

In Manifest, property names with multiple words are separated with underscore (_) and values are separated with dash (-).

Short name is recommended to be less than 12 characters, so user can see the whole name.


In the HTML we need to add link: 
```html
<link rel="manifest" href="app.webmanifest?1">
```

Always save Web manifest file in the root directory.  
We can have multiple PWAs in the same domain. In this case each Web manifest files should be in different folders.

### Start URL 

The start domain must be within the same origin as Web App Manifest.

```"start_url": "./"``` - ```./``` means whatever the root folder is.

The start url can have some arguments too (optional):
```json
  "start_url": "./?utm_source=pwa",
```
E.g. We can add Google analytics attribute to know when the user is using browser or the app from the icon.





## Mandatory properties for webmanifest:
```json
{
  "name": "Codepad Masters, the best PWA in town",
  "short_name": "Codepad",
  "start_url": "./"
}
```



#### Theme color

When we set a theme color, we also set a meta tag in HTML (!HTML uses dash (theme-color)).
```json
  "theme_color": "orange"
```
```html
  <meta name="theme-color" content="#ffc252">
```

Meta tag also accepts 'media=""' attribute, e.g. for light and dark bg. But having multiple theme colors is not possible in web manifest.

```<meta>``` tag in the HTML is used for the theme color. Web App Manifest property is used as fallback. It uses fallback color before HTML is loaded.


### Updating the manifest

E.g. if we want to change icon:

- On Android the change will be reflected the next day. The installed PWA will be recreated and reinstalled with the new values on the next day.
- On iOS and desktop, those values won't be updated. The user needs to reinstall the PWA.


### Scope

"scope" property defines what is within the app and what is outside the app.  
Each domain, even if it is a sub-domain, will be treated as external origin in PWA. Even if we are pointing to the same manifest. They will be different PWA.

In case of authentications, like OAuth, it will open an inner browser.

### Display

```"display"``` can have two values:
- ```"standalone"``` - We want our app to be rendered as stand alone application (most commonly used).
- ```"browser"```less common. The website is treated as bookmark - a shortcut to the browser (not commonly used).
- ```"minimal-ui"``` - Will ask the browser to render a minimal navigation user interface (it has stop❌, reload🔄️ and back⬅️ buttons at the top). iOS does not support ```"minimal-ui"```, only ```"standalone"```. If we set ```"minimal-ui"``` on iOS, it will open the browser.
- ```"fullscreen"``` - available only on android. Desktop and iOS will fall back to ```"standalone"```.

```json
  "display": "standalone"
```


### Icons

It is an array of icons.  
We need to specify the size. The spec does not recommend specifying the size, but in real world it is necessary to specify it, so always specify the size.  Minimal size we need is at least 5x12/5x12. It must be square and don't add rounded corners to the PNG (in Safari).

We need to specify the image type. At this moment, only PNG is widely supported. SVG (also needs to specify sizes too) still needs a fallback  to the PNG (so we need to set two icons, one for SVG and one for PNG). SVGs are only available on latest versions of Google Chrome and is coming on Android. Icons can be in a CDS, it can point to the external origin.



### Rendering

iOS and Firefox have less strict PWA criterias. At this point we can install the PWA: 
```json
{
  "name": "Codepad Masters, the best PWA in town",
  "short_name": "Codepad",
  "start_url": "./",
  "orientation": "all",
  "theme_color": "#ffc252",
  "scope": "./",
  "display": "standalone",
  "icons": [
    {
      "src": "icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    },
    {
      "src": "icons/icon-1024.png",
      "sizes": "1024x1024",
      "type": "image/png"
    }
  ]
}
```


Safari ignores ```"icons"``` in Manifest.  
To solve this problem, we need to set this link tag in the HTML:
```html
    <link rel="apple-touch-icon" href="icons/icon-512.png">
```


#### Trick: How to check how does our app looks without passing the PWA criteria yet

This trick is not available on Android.

In Chrome -> ... -> More Tools -> Create Shortcut


### Icons on Android


#### Shortcuts on Android

- Creates a shortcut to browser's engine
- Browser's badge (Android 8+)
- It's installed only at the home screen
- No icon in the launcher
- It doesn't appear at app's list
- All browsers use this method

#### WebAPK on Android

- Only if PWA Criteria passes
- It's a full Android native package
- It contains only app's name, icon and URL
- APK is installed silently
- Icon goes to home screen and launcher
- Google Chrome with Play Services
- Samsung Internet on Samsung devices
- It is not available on any other browsers except Google Chrome and Samsung.


### Icons for the App Manifest

- Format: PNG, Color space sRGB
- Used on Android and desktop OS
- If there is no exact icon available it will pick
- the closest one
- Recommended sizes:
  - at least: 192x192, 512x512
  - 384x384, 1024x1024
  - deprecated sizes: 72x72, 152x152
  - Simpler versions: 96x96, 144x144


#### Maskable Icon

For icons on Android, they might be displayed in circle, or square or in many different shapes. So the icon should be adaptable for all those cases.

To solve this problem, we can provide a Maskable Icon.
It's still a square icon but with enough discardable padding content out of a safe area. 

We render central circular space with 40% raduis. Everything else is outside the "safe space".  
In the Webmanifest, we need to add ```"purpose": "maskable"``` to the icon and we need to check whether our icon works well or not (we can check on this website: https://maskable.app/. We can resize the icon on this website too).



### Disable selection

We can (should) disable selection on some parts of our app, like buttons (user doesn't need to copy e.g. a delete button).  
We can do it from css:
```css
  user-select: none;
  -webkit-user-select: none;
```


#### Unsafe zone in the phones

When we are in the standalone application, we shouldn't have interactive parts at the bottom, where OS buttons (multitask, go right/left) are, otherwise, they will cover that part of an app.

One way to solve this problem is media-queries in CSS (not the best solution):
```css
@media (display-mode: standalone) {
  body>toolbar {
    padding-bottom: 32px;
  }
}
```

Better solution - CSS Environmental Properties. In this case on all sides:
```css
@media (display-mode: standalone) {
  body>toolbar {
        padding: env(safe-area-inset-top) 
            env(safe-area-inset-right) 
            env(safe-area-inset-bottom, 5px) 
            env(safe-area-inset-left) !important;
  }
}
```

```env(safe-area-inset-bottom, 5px) ``` means if there is no default margin, use 5px.

On iPhone, we don't have support for 100% of viewport and we can request it (we can have 100% of the screen on landscape mode and will add margin at the bottom): 
```html
    <meta name="viewport" content="width=device-width,viewport-fit=cover">
```