# PimpMyStremio Internal Modules

A list of modules that are also allowed to be required in local add-ons, but are handled internally by PimpMyStremio, as opposed to the [whitelisted modules](./modules-whitelist.md).

## Cinemeta

Gets movie / series metadata from Cinemeta based on IMDB ID and type (`movie` or `series`)

```javascript
const cinemeta = require('cinemeta')

cinemeta.get({ imdb: 'tt0944947', type: 'series' }).then(resp => {
  // resp is Game of Thrones metadata
}).catch(err => {
  // error
})
```

## Eval

A safer `eval`, as the default one is disabled in the sandbox.

```javascript
const evl = require('eval')

evl('Math.random()').then(resp => {
  // resp is 0.8169928584181472
}).catch(err => {
  // error
})
```

## Proxy

The proxy has a synchronous API that allows you to change headers of URLs on the fly, disabling CORS, referrer checks, etc.

```javascript
const { proxy } = require('internal')

const proxiedURL = proxy.addProxy('https://www.imdb.com/title/tt0944947/', { headers: { referer: 'https://www.imdb.com/' } })
```

## PhantomJS

This is an internal function that uses [phantom](https://www.npmjs.com/package/phantom) under the hood.

```javascript
const phantom = require('phantom')

phantom.load(options, clientArguments, phantomOptions, (instance, page) => {
  // ..
})
```

In this example code:
- `options` - refers to an object of internal options, supports `agent` (string) for user agent, `headers` for an object of custom headers, `noRedirect` (boolean) to block the page from redirecting and `clearMemory` (boolean) to not use cached data when loading the page
- `clientArguments` is an array of strings that includes the client arguments to pass to the phantomjs binary (as used in [phantom](https://www.npmjs.com/package/phantom)), default is: `["--ignore-ssl-errors=true", "--web-security=false", "--ssl-protocol=tlsv1", "--load-images=false", "--disk-cache=true"]`
- `phantomOptions` is an object of options as used in [phantom](https://www.npmjs.com/package/phantom), default is: `{ logLevel: 'warn' }`

**Warning: always close your phantomjs page when you're done, like so:**

```javascript
phantom.close(instance, page, callback)
```

## Stremio Add-on SDK

The add-on sdk, although exactly like [stremio-addon-sdk](https://github.com/Stremio/stremio-addon-sdk#readme), is considered an internal module because it's actually a shim of the actual SDK.

While all of the same rules apply as with the original SDK, the only difference is that you need to have the sdk code in `index.js` in your root folder, and it must end with `module.exports = getRouter(addonInterface)`.

`index.js` example:

```javascript
const { addonBuilder, getInterface, getRouter } = require('stremio-addon-sdk')

const builder = new addonBuilder(manifest)

builder.defineCatalogHandler(args => {
  // ..
})

builder.defineMetaHandler(args => {
  // ..
})

builder.defineStreamHandler(args => {
  // ...
})

const addonInterface = getInterface(builder)

module.exports = getRouter(addonInterface)
```

## Config

This internal module is [described here](https://github.com/sungshon/PimpMyStremio/tree/master/docs#user-settings)

## Persisted Data

This internal module is [described here](https://github.com/sungshon/PimpMyStremio/tree/master/docs#persisted-data)




