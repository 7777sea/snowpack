---
layout: layouts/extend.njk
---


#### Who is this page for?

- Anyone writing a custom plugin for Snowpack.
- Anyone extending Snowpack's default behavior.
- Anyone adding framework-specific auto-HMR.
- Anyone using Snowpack programatically (ex: `snowpack.install()`).

Looking for more basic help using Snowpack in your project?  
👉 **[Check out our main user guide.](/)**

## Plugin API

### Overview

```ts
// TODO: More documentation coming soon.
interface SnowpackPlugin {
  // knownEntrypoints - additional web_modules to install in the project
  knownEntrypoints?: string[],
  // defaultBuildScript - a build script to configure for the user automatically
  defaultBuildScript?: string,
  // build() - Build any source file to web-ready JS, CSS, or HTML
  build?: ({
    filePath: string,
    contents: string,
    isDev: boolean,
  }) => Promise<{result: string; resouces?: {css?: string}}>
  // transform() - Transform JS, CSS, or HTML
  transform?: ({
    urlPath: string,
    contents: string,
    isDev: boolean,
  }) => Promise<{result: string}>
  // bundle() - Bundle the web application for production.
  bundle?(args: {
    srcDirectory: string;
    destDirectory: string;
    jsFilePaths: Set<string>;
    log: (msg) => void;
  }): Promise<void>;
}
```

### Example 
```js
// Example: This plugin adds automatic HMR for Preact applications.
module.exports = function plugin(snowpackConfig, pluginOptions) {
  return {
    /**
     * knownEntrypoints - Additional web_modules to install in the project
     * that may not otherwise be picked up by the install command
     */
    knownEntrypoints: ['@prefresh/core'],
    /**
     * build() - Build any source file to web-ready JS, CSS, or HTML.
     * Not needed by this plugin, so really its safe to remove entirely.
     */
    async build({ contents, filePath, isDev }) {},
    /**
     * transform() - Transform web assets (JS, CSS, or HTML). Useful for 
     * post-processing or adding functionality into your web app.
     */
    async transform({ contents, urlPath, isDev }) {
      if (!isDev) {
        return;
      }
      if (!urlPath.endsWith('.js')) {
        return;
      }
      return {
        result: `
          import '@prefresh/core';
          ${contents}

          if (import.meta.hot) {
            /* IMPLEMENTATION, REMOVED FOR BREVITY */
          }`,
      };
    },
  };
};
```


## HMR API

### Overview

```js
// TODO: More documentation coming soon.

if (import.meta.hot) {
  import.meta.hot.accept(({module}) => {
    // All module updates are decoupled from the application to start.
    // Do something here to connect new updated modules into the main,
    // app-connected module. Usually, this involves updating your 
    // exports to point to the exports of the new `module` object.
  });
  import.meta.hot.dispose(() => {
    // Cleanup anything that could cause side-effects.
    // Called once a new module version has been loaded.  
  });
}
```

## JavaScript API

### Overview


```js
// TODO: More documentation coming soon.
```
