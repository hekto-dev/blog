---
title: "Jest ModuleNameMapper"
date: 2022-12-01T10:00:00
author: Sebastian Korfmann
tags:
  - jest
  - "#til"
---

Had to deal with some ESM / CJS madness again.

<!--more-->

## Jest ModuleNameMapper

The AWS SDK relies on [uuid](https://github.com/uuidjs/uuid) `8.3` - which doesn't [play well](https://stackoverflow.com/a/73203803/260085) with Jest. I was a bit surprised that the usual [transformIgnorePatterns](https://jestjs.io/docs/configuration#transformignorepatterns-arraystring) didn't work. Always resulted in the following error:

```
node_modules/.pnpm/uuid@8.3.2/node_modules/uuid/dist/esm-browser/index.js:1
    ({"Object.<anonymous>":function(module,exports,require,__dirname,__filename,jest){export { default as v1 } from './v1.js';
                                                                                      ^^^^^^

    SyntaxError: Unexpected token 'export'
```


The following [moduleNameMapper](https://jestjs.io/docs/configuration#modulenamemapper-objectstring-string--arraystring) config made it work:

```
 moduleNameMapper: {
    // Force module uuid to resolve with the CJS entry point, because Jest does not support package.json.exports. See https://github.com/uuidjs/uuid/issues/451
    "uuid": require.resolve('uuid'),
  },
```