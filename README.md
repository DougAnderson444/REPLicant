# REPLicant... with MDSVEX  

This is a repro fork of [REPLicant](https://github.com/pngwn/REPLicant), which shows you how to make a Svelte REPL.

But what about a simple mdsvex REPL in the browser?

Turns out it's possible, the only thing I can't get ot work is the code syntax highlighting. Which is why this repro(ducable example) exists.

# Reproducable example of adding mdsvex to REPLicant

Changes:
```js
// in App.svelte
App.svelte > App.svx // chg to mdsvex format
```

In the worker, import the UMD distribution for mdsvex. I tried using the es export or compiling to iife, but get `require` (not defined) or `import/export` (only in modules) type o errors.


```js
// in worker.ts
importScripts(`${CDN_URL}/mdsvex/dist/mdsvex.js`) // import the mdsvex worker

// chg to mdsvex format
input: "./App.svx", 

// add mdsvex preprocessor
if (/\.svx$/.test(id)) {
  preprocessPromise = self.mdsvex
  .mdsvex()
  .markup({ content: code, filename: id })
}

```
In rollup config:

```js
// rollup.config.js
// add node globals for process.browser in mdsvex.js
import globals from 'rollup-plugin-node-globals' // !(process).browser for the mdsvex umd distribution
plugins: [
  ...
  globals(), // need to add for mdsvex !(process ).browser
  ]
```

## Questions

Why doesn't adding the browser version of mdsvex.mdsvex().markup() highlight the code?

There are no `class="token comment"` in the code, why not?
