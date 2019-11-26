# ClojureScript externs woes

This repo reproduces a snag I just tripped on. I'm not sure if this is a bug or
the exepected behavior.

This repo includes a single namespace, `link.core`. It also includes an external
script that defines a function `window.link.myfn`. There is an externs file that
informs the Closure compiler of the external script. If you run this in
development, everything works as expected:

```sh
clojure -A:dev:repl
```

Then open [http://localhost:9110/](http://localhost:9110/) and observe two
printed messages in the console - one from ClojureScript and one from the
external script being called from the ClojureScript namespace.

Now try to build this with advanced compilation settings:

```sh
clojure -A:prod
```

Open `resources/public/index-prod.html` and observe that the `link` object is
now being overwritten by the app bundle - with an empty object. Result: prod
build does not work.
