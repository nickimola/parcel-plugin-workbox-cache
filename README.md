# parcel-plugin-workbox-cache

![NPM](https://img.shields.io/npm/l/parcel-plugin-workbox-cache?style=flat-square)
![npm](https://img.shields.io/npm/v/parcel-plugin-workbox-cache?style=flat-square)
![npm](https://img.shields.io/npm/dt/parcel-plugin-workbox-cache?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/nickimola/parcel-plugin-workbox-cache?style=flat-square)
![GitHub watchers](https://img.shields.io/github/watchers/nickimola/parcel-plugin-workbox-cache?style=flat-square)
![GitHub forks](https://img.shields.io/github/forks/nickimola/parcel-plugin-workbox-cache?style=flat-square)

An updated plugin for [Parcel][parcel url] to generate a service worker with [Workbox][workbox url], derived from Anders Dahnielson's [parcel-plugin-workbox][ppw url].

## Install

You can either install by running yarn (recommended)

```bash
yarn add parcel-plugin-workbox-cache --dev
```

or use npm

```bash
npm install parcel-plugin-workbox-cache --save-dev
```

## Usage

When you build resources with Parcel, the plugin will generate a service worker `sw.js` and insert it into your project's `index.html` entry file.

You can customize the settings by adding a `.workbox-config.js` file in the root of your project. This will be the settings that will be passed to `generateSW()`function found in workbox-build dependency. If you don't create any config file, default settings will be passed:

```js
    {
      importScripts: ['./worker.js'],
      globDirectory: './dist'
      globPatterns: [`**/*.{css,html,js,gif,ico,jpg,png,svg,webp,woff,woff2,ttf,otf}`]
    }
```

Here's an example `.workbox-config.js` file

```js
module.exports = {
  importScripts: [],
  globDirectory: "./dist",
  globPatterns: [
    "**/*.{css,html,js,gif,ico,jpg,png,svg,webp,woff,woff2,ttf,otf,eot,webmanifest,manifest}"
  ],
  runtimeCaching: [
    {
      urlPattern: new RegExp("^https:\/\/firebasestorage\\.googleapis\\.com\/.*", "gi"),
      handler: "CacheFirst",
      options: {
        cacheableResponse: {
          statuses: [0, 200]
        },
        cacheName: "images"
      }
    }
  ],
  offlineGoogleAnalytics: true
};
```

By default, a `worker.js` file, where you can write the logic for your service worker, will be read from your project's root directory and imported into `sw.js` unless you change this setting. Additionally, a CDN version of Google Workbox is imported automatically.

Note: If you have this plugin active during development, you will need to either disable or manually delete the service worker to bypass caching.

[parcel url]: https://parceljs.org
[workbox url]: https://developers.google.com/web/tools/workbox/
[ppw url]: //github.com/dahnielson/parcel-plugin-workbox
