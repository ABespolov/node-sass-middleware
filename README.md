# node-sass-middleware

Connect/Express middleware for [node-sass](https://github.com/sass/node-sass).

## Install

    npm install node-sass-middleware-5

## Usage

Recompile `.scss` or `.sass` files automatically for connect and express based http servers.

### Connect example

```javascript
var connect = require('connect')
var sassMiddleware = require('node-sass-middleware-5')
var server = connect.createServer(
  sassMiddleware({
      /* Options */
      src: __dirname
    , dest: __dirname + '/public'
    , debug: true
    , outputStyle: 'compressed'
    , prefix:  '/prefix'  // Where prefix is at <link rel="stylesheets" href="prefix/style.css"/>
  }),
  connect.static('/prefix', __dirname + '/public')
);
```

Heavily inspired by <https://github.com/LearnBoost/stylus>

### Express example

```javascript
var express = require('express');
var sassMiddleware = require('node-sass-middleware');
var path = require('path');
var app = express();
app.use(sassMiddleware({
    /* Options */
    src: __dirname,
    dest: path.join(__dirname, 'public'),
    debug: true,
    outputStyle: 'compressed',
    prefix:  '/prefix'  // Where prefix is at <link rel="stylesheets" href="prefix/style.css"/>
}));
// Note: you must place sass-middleware *before* `express.static` or else it will
// not work.
app.use('/public', express.static(path.join(__dirname, 'public')));
```

### Connect with other middleware example

```javascript
var connect = require('connect');
var sassMiddleware = require('node-sass-middleware');
var postcssMiddleware = require('postcss-middleware');
var autoprefixer = require('autoprefixer');
var path = require('path');
var http = require('http');
var app = connect();
var destPath = __dirname + '/public';
app.use(sassMiddleware({
    /* Options */
    src: __dirname
  , response: false
  , dest: destPath
  , outputStyle: 'extended'
}));
app.use(postcssMiddleware({
  plugins: [
    /* Plugins */
    autoprefixer({
      /* Options */
    })
  ],
  src: function(req) {
    return path.join(destPath, req.url);
  }
}));

http.createServer(app).listen(3000);
```

### Options

 *    `src`            - (String) Source directory used to find `.scss` or `.sass` files.

#### Optional configurations:

 *    `beepOnError`    - Enable beep on error, false by default.
 *    `debug`          - `[true | false]`, false by default. Output debugging information.
 *    `dest`           - (String) Destination directory used to output `.css` files (when undefined defaults to `src`).
 *    `error`          - A function to be called when something goes wrong.
 *    `force`          - `[true | false]`, false by default. Always re-compile.
 *    `indentedSyntax` - `[true | false]`, false by default. If true compiles files with the `.sass` extension instead of `.scss` in the `src` directory.
 *    `log`            - `function(severity, key, val, message)`, used to log data instead of the default `console.error`. "severity" matches [Winston](https://www.npmjs.com/package/winston) severity levels.
 *    `maxAge`         - MaxAge to be passed in Cache-Control header.
 *    `prefix`         - (String) It will tell the sass middleware that any request file will always be prefixed with `<prefix>` and this prefix should be ignored.
 *    `response`       - `[true | false]`, true by default. To write output directly to response instead of to a file.
 *    `root`           - (String) A base path for both source and destination directories.


  For full list of options from original node-sass project go [here](https://github.com/sass/node-sass).

### Express example with custom log function

```javascript
var express = require('express');
var sassMiddleware = require('node-sass-middleware');
var path = require('path');
var winston = require('winston');
var app = express();
winston.level = 'debug';
app.use(sassMiddleware({
    /* Options */
    src: __dirname,
    dest: path.join(__dirname, 'public'),
    debug: true,
    log: function (severity, key, value) { winston.log(severity, 'node-sass-middleware   %s : %s', key, value); }
}));
// Note: you must place sass-middleware *before* `express.static` or else it will
// not work.
app.use(express.static(path.join(__dirname, 'public')));
```

## Contributors

We <3 our contributors! A special thanks to all those who have clocked in some dev time on this project, we really appreciate your hard work. You can find [a full list of those people here](https://github.com/sass/node-sass-middleware/graphs/contributors).

### Building and Testing

```sh
git clone git@github.com:sass/node-sass-middleware
cd node-sass-middleware

npm install
npm test
```

### Note on Patches/Pull Requests

 * Fork the project.
 * Make your feature addition or bug fix.
 * Add documentation if necessary.
 * Add tests for it. This is important so I don't break it in a future version unintentionally.
 * Send a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2013+ Andrew Nesbitt. See [LICENSE](https://github.com/sass/node-sass-middleware/blob/master/LICENSE) for details.
