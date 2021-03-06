# mercury-jsxify

[Browserify][] transform for JSX (superset of JavaScript used in [React][]
library):

    /**
     * @jsx h
     */

    var h = require("mercury").h

    function hello(name) {
      return <div>Hello, {name}!</div>
    }

    mercury.app(
      document.getElementById('hello'),
      mercury.value('world'),
      hello
    )

Save the snippet above as `main.js` and then produce a bundle with the following
command:

    % browserify -t mercury-jsxify main.js

`mercury-jsxify` transform activates for files with either `.jsx` extension or `/**
@jsx React.DOM */` pragma as a first line for any `.js` file.

If you want to mercury-jsxify modules with other extensions, pass an `-x /
--extension` option:

    % browserify -t coffeeify -t [ mercury-jsxify --extension coffee ] main.coffee

If you don't want to specify extension, just pass `--everything` option:

    % browserify -t coffeeify -t [ mercury-jsxify --everything ] main.coffee

## ES6 transformation

`mercury-jsxify` transform also can compile a limited set of es6 syntax constructs
into es5. Supported features are arrow functions, rest params, templates, object
short notation and classes. You can activate this via `--es6` or `--harmony`
boolean option:

    % browserify -t [ mercury-jsxify --es6 ] main.js

You can also configure it in package.json

```json
{
    "name": "my-package",
    "browserify": {
        "transform": [
            ["mercury-jsxify", {"es6": true}]
        ]
    }
}
```

## Using 3rd-party jstransform visitors

mercury-jsxify uses [jstransform][] to transform JavaScript code. It allows code
transformations to be pluggable and, what's more important, composable. For
example JSX and es6 are implemented as separate code transformations and still
can be composed together.

mercury-jsxify provides `--visitors` option to specify additional jstransform visitos
which could perform additional transformations.

It should point to a module which exports `visitorList` attribute with a list of
transformation functions to be applied:

    % browserify -t [ mercury-jsxify --visitors es6-module-jstransform/visitors ] main.js

Example above uses [es6-module-jstransform][] to compile es6 module syntax
(`import` and `export` declarations) into CommonJS module constructs.

[Browserify]: http://browserify.org
[React]: http://facebook.github.io/react/
[jstransform]: https://github.com/facebook/jstransform
[es6-module-jstransform]: https://github.com/andreypopp/es6-module-jstransform
