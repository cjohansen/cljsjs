# ClojureScript externs woes

This repo reproduces a snag I just tripped on. I'm not sure if this is a bug or
the exepected behavior.

This repo includes a single namespace, `link.core`. It also includes an external
script that defines a function `window.link.myfn`. There is an externs file that
informs the Closure compiler of the external script.

## Development: works

If you run this in development, everything works as expected:

```sh
clojure -A:figwheel:build-dev
open resources/public/index.html
```

Now you can observe two printed messages in the console - one from ClojureScript
and one from the external script being called from the ClojureScript namespace.

## Production: no go

Now try to build this with advanced compilation settings:

```sh
clojure -A:figwheel:build-prod
open resources/public/prod.html
```

Open `resources/public/index-prod.html` and observe that the `link` object is
now being overwritten by the app bundle - with an empty object. Result: prod
build does not work.

## Without the externs

Interestingly, if you drop the call to `(js/link.myfn)` and compile without the
externs, no global `link` object is created at all:

```sh
clojure -A:figwheel:build-prod-noext
open resources/public/noext.html
```

## Renaming the namespace

If you build the production build as before, but do not use overlapping names on
the externed function and the namespace, things work as expected:

```sh
clojure -A:figwheel:build-other-ns
open resources/public/otherns.html
```
