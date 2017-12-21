# PRPL with Custom Elements and Firebase

These are the resource files needed for the [PRPL with Custom Elements and Firebase](https://codelabs.developers.google.com/codelabs/prpl-ce-firebase/) codelab.

In this codelab, you’ll learn how to build a web app with Custom Elements that implements the PRPL pattern and deploy to Firebase Hosting.

** if you define a contructor() for a custom element, must call super() in the constructor() before accessing this so that the browser can establish the correct prototype chain **

> workbox-cli generate:sw
modify a precached asset, you must re-generate the service worker using the above command

`workbox-cli generate:sw` to `workbox generate:sw` changed from workbox-cli to workbox since workbox-cli will stop working


`Link: rel=preload` specify in HTTP header for server push. Without server push, this response header tells the browser that it should begin downloading this resource before processing the page.
you might consider not pushing a resource to save data (which can be done by specifying `Link: rel=preload; nopush` if it's likely that the resource is already cached

For HTTP2 Server push in firebase, add `headers` in `hosting` object to which files should be requested before loading the page
```
{
  "hosting": {
    "public": "public",
    "rewrites": [
      {
        "source": "/detail/*",
        "destination": "/index.html"
      }
    ],
    "headers": [
      {
        "source": "/",
        "headers": [{
          "key": "Link",
          "value": "</elements/my-app.js>;rel=preload;as=script,</style.css>;rel=preload;as=style,</elements/list-view.js>;rel=preload;as=script,</data/list.json>;rel=preload"
        }]
      },
      {
        "source": "/detail/*",
        "headers": [{
          "key": "Link",
          "value": "</elements/my-app.js>;rel=preload;as=script,</style.css>;rel=preload;as=style,</elements/detail-view.js>;rel=preload;as=script"
        }]
      }
    ]
  }
}
```
