# Installation

You can install the FunnelQL Javascript library using NPM or by downloading the files manually from [Github](https://github.com/FunnelQL/funnelql/) or [Gitlab](https://gitlab.com/FunnelQL/funnelql).

## Node Package Manager

You can install the Javascript library in your project using the following NPM command.

```bash
npm install funnelql
```

You can then use the FunnelQL library with the file `dist/funnelql.js` or `dist/funnelql+inline-worker.js`. To save bytes, you can optionally use the `.nodebug.js` version without debug and Performance API functionality.

## Including the library

The Javascript library can be included like any regular javascript file, is safe to include in concatenation and can be loaded asynchronously. 

The library is completely written in [Vanilla JS](http://vanilla-js.com/) (original Javascript) and does not depend on other projects.

The library requires a Web Worker that can be loaded in two ways: 

1. as a inline Javascript [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob) 
2. as a separate worker file `funnelql-worker.js`. 

### Option 1: library with a separate web worker file

You can directly include the FunnelQL library in the HTML document.

```html
<script src="/funnelql.js" async></script>
```

The script will automatically load `/funnelql-worker.js` on the basis of the path of `funnelql.js`. 

To load the web worker from a custom location, set `window['funnelql-worker']` with the path to the web worker file.

### Option 2: library with a inline web worker blob

```html
<script src="/funnelql+inline-worker.js" async></script>
```

Once the FunnelQL library is loaded you can access the Javascript API via the global variable `$FQL` (see [Javascript API](javascript-api.md))

