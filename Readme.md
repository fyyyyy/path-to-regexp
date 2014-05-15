# Path-to-RegExp

Turn an Express-style path string such as `/user/:name` into a regular expression.

[![Build Status](https://img.shields.io/travis/component/path-to-regexp/master.svg)](https://travis-ci.org/component/path-to-regexp)
[![NPM version](https://img.shields.io/npm/v/path-to-regexp.svg)](https://www.npmjs.org/package/path-to-regexp)

## Usage

```javascript
var pathToRegexp = require('path-to-regexp');

// pathToRegexp(path, keys, options);
```

- **path** A string in the express format, an array of strings, or a regular expression.
- **keys** An array to be populated with the keys present in the url.
- **options**
  - **options.sensitive** When set to `true` the route will be case sensitive.
  - **options.strict** When set to `true` a slash is allowed to be trailing the path.
  - **options.end** When set to `false` the path will match at the beginning.

```javascript
var keys = [];
var re = pathToRegexp('/foo/:bar', keys);
// re = /^\/foo\/([^\/]+?)\/?$/i
// keys = ['bar']
```

### Parameters

The path has the ability to define parameters and automatically populate the keys array.

#### Named Parameters

Named parameters are defined by prefixing a colon to the parameter name (`:foo`). By default, this parameter will match up to the next path segment.

```js
var re = pathToRegexp('/:foo/:bar');
// keys = ['foo', 'bar']

re.exec('/test/route');
//=> ['/test/route', 'test', 'route']
```

#### Optional Parameters

Optional parameters can be denoted by suffixing a question mark. This will also make any prefixed path segment optional (`/` or `.`).

```js
var re = pathToRegexp('/:foo/:bar?');
// keys = ['foo', 'bar']

re.exec('/test');
//=> ['/test', 'test', undefined]

re.exec('/test/route');
//=> ['/test', 'test', 'route']
```

#### Custom Matches

All parameters can be provided a custom matching regexp and override the default. Please note: Backslashes need to be escaped in strings.

```js
var re = pathToRegexp('/:foo(\\d+)');

re.exec('/123');
//=> ['/123', '123']

re.exec('/abc');
//=> null
```

#### Unnamed Parameters

It is possible to write an unnamed parameter that is only a matching group. It works the same as a named parameter, except it will be numerically indexed.

```js
var re = pathToRegexp('/:foo/(.*)', keys);
// keys = ['foo', '0']

re.exec('/test/route');
//=> ['/test/route', 'test', 'route']
```

## Live Demo

You can see a live demo of this library in use at [express-route-tester](http://forbeslindesay.github.com/express-route-tester/).

## License

MIT
