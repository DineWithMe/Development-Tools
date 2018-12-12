# Create Next Progressive Web App

For this tutorial, basically there is 3 parts:

- preparing manifest and icons
- register service worker
- Publish to App Store (not ready yet)

## Part 1 Manifest and Icons

Manifest tell the browser how to install your app.

Read more here:  
https://developers.google.com/web/fundamentals/web-app-manifest/

https://developers.google.com/web/fundamentals/app-install-banners/#criteria

### 1. Generate manifest and icons

Go to https://realfavicongenerator.net/ and create your manifest and icons, this website is one of the easiest to use. (Edit: actually https://www.pwabuilder.com/ is better)

You will have some option along the way, configure it if you want.

![].(img/favicon generator.png)

You will see something like this, download the package.

### 2 Copy into the project

Unzip the code and copy all the files into your project `static` folder, I recommend `static/public`.

In your \_document.js, paste the code in `Head` and refactor the path.

This is JSX, dont forget to self-closed the tag.

```
<Head>
  <link rel="apple-touch-icon" sizes="180x180" href="/static/public/apple-touch-icon.png"/>
  <link rel="icon" type="image/png" sizes="32x32" href="/static/public/favicon-32x32.png"/>
  <link rel="icon" type="image/png" sizes="16x16" href="/static/public/favicon-16x16.png"/>
  <link rel="manifest" href="/static/public/site.webmanifest"/>
  <link rel="mask-icon" href="/static/public/safari-pinned-tab.svg" color="#5bbad5"/>
  <meta name="msapplication-TileColor" content="#da532c"/>
  <meta name="theme-color" content="#ffffff"/>
</Head>
```

### 3. Modify the manifest file

Go to your `static/public` folder and open the `site.webmanifest`

```
{
  "name": "MyApp",
  "short_name": "MyApp",
  "icons": [
    {
      "src": "/static/public/android-chrome-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/static/public/android-chrome-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "start_url": "/",
  "theme_color": "#ffffff",
  "background_color": "#ffffff",
  "display": "standalone",
  "scope":"/",
  "description":"myApp"
}

```

Replace the content with above code and choose any name and color you like and we are done with manifest and icons.

Read more:  
https://developers.google.com/web/fundamentals/web-app-manifest/

## Part 2 Service Worker

Service worker cache your website and allows it to run offline.

Read more here:  
https://medium.com/@anatomic/using-a-service-worker-with-next-js-460e0168a60a

https://www.youtube.com/watch?v=ksXwaWHCW6k

### 1. Register service worker

```
  componentDidMount() {
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('/serviceWorker.js').catch((err) =>
        console.error('Service worker registration failed', err)
      )
    } else {
      console.log('Service worker not supported')
    }
  }
```

Copy this code to your \_app.js component.

### 2. Listen to activate and fetch event

```
const cacheName = 'v1'

// Activate Event
self.addEventListener('activate', (e) => {
  e.waitUntil(
    caches.keys().then((cacheNames) => {
      cacheNames.map((cache) => {
        if (cache !== cacheName) {
          // delete old cache
          return caches.delete(cache)
        }
      })
    })
  )
})

// Fetch Event
self.addEventListener('fetch', (e) => {
  e.respondWith(
    fetch(e.request)
      .then((res) => {
        // Make a copy of response
        const resClone = res.clone()
        // open cache
        caches.open(cacheName).then((cache) => {
          // add response to cache, e.request as key and resClone as value
          cache.put(e.request, resClone).catch(console.log)
        })
        return res
      })
      .catch(() => caches.match(e.request).then((res) => res))
  )
})
```

Create a serviceWorker.js in your src folder and copy the code. What the code does is:
-delete old version cache during activate event and
-if online, cache the fetch respond; if offline, get the data from cache.

### 3. Implementing custom server

```
const { createServer } = require('http');
const { parse } = require('url');
const { createReadStream } = require('fs');
const next = require('next');

const dev = process.env.NODE_ENV !== 'production';
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  createServer((req, res) => {
    const parsedUrl = parse(req.url, true);
    const { pathname } = parsedUrl;

    if (pathname === '/serviceWorker.js') {
      res.setHeader('content-type', 'text/javascript');
      createReadStream('./offline/serviceWorker.js').pipe(res);
    } else {
      handle(req, res, parsedUrl);
    }
  }).listen(3000, err => {
    if (err) throw err;
    console.log('> Ready on http://localhost:3000');
  });
});
```

Create a server.js in root directory and copy the code, we need to read the serviceWorker from server directory because serviceWorker.js is not automatically included in the client side.
