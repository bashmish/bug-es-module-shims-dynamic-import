# bug-es-module-shims-dynamic-import

> See open issue in es-module-shims https://github.com/guybedford/es-module-shims/issues/471

Dirs "es-module-shims-v1" and "es-module-shims-v2" are identical, except a different version of the polyfill is loaded.

Steps to reproduce:

```sh
npm install
```

```sh
npm start
```

Install Firefox 107 from https://ftp.mozilla.org/pub/firefox/releases/107.0/ (or other browser without native import maps).

Open in Firefox 107:

- http://127.0.0.1:8080/es-module-shims-v1/scenario1/index.html
- http://127.0.0.1:8080/es-module-shims-v1/scenario2/index.html
- http://127.0.0.1:8080/es-module-shims-v1/scenario3/index.html
- http://127.0.0.1:8080/es-module-shims-v1/scenario4/index.html
- http://127.0.0.1:8080/es-module-shims-v2/scenario1/index.html
- http://127.0.0.1:8080/es-module-shims-v2/scenario2/index.html
- http://127.0.0.1:8080/es-module-shims-v2/scenario3/index.html
- http://127.0.0.1:8080/es-module-shims-v2/scenario4/index.html

Expected:

- "scenario2" works
- "scenario4" works not only in `es-module-shims-v1`, but also in `es-module-shims-v2`

Actual:

- "scenario2" doesn't work, seems like a static import is necessary before the dynamic import to make it work (later shown in "scenario3" and "scenario4")
- "scenario4" works in `es-module-shims-v1`, but not in `es-module-shims-v2`, seems like a regression in v2

"scenario3" and "scenario4" seem like a workaround, but it's probably needed due to how the polyfill is implemented.

"scenario4" is the closest to reality of my project where we have a chain of static imports and then a dynamic import for features hosted on the CDN.
