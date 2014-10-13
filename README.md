# transducers-js

A high performance
[Transducers](http://blog.cognitect.com/blog/2014/8/6/transducers-are-coming)
implementation for JavaScript.

Transducers are composable algorithmic transformations. They are
independent from the context of their input and output sources and
specify only the essence of the transformation in terms of an
individual element. Because transducers are decoupled from input or
output sources, they can be used in many different processes -
collections, streams, channels, observables, etc. Transducers compose
directly, without awareness of input or creation of intermediate
aggregates.

For further details about Transducers see the following resources:
* ["Transducers are coming" announce blog post](http://blog.cognitect.com/blog/2014/8/6/transducers-are-coming)
* [Rich Hickey's Transducers StrangeLoop presentation](https://www.youtube.com/watch?v=6mTbuzafcII)

## Releases and Dependency Information

* Latest release: 0.4.113

### JavaScript

You can include either the [release](http://cdn.cognitect.com/transducers/transducers-0.4.113-min.js) (2K gzipped) or [development](http://cdn.cognitect.com/transducers/transducers-0.4.113.js) build of transducers-js on your webpage. We also provide [Require.js](http://requirejs.org) compatible [release](http://cdn.cognitect.com/transducers/transducers-0.4.113-amd-min.js) and [dev](http://cdn.cognitect.com/transducers/transducers-0.4.113-amd.js) builds.

### Node.js

transducers-js is released to [npm](https://www.npmjs.org). Add transducers-js to your `package.json` dependencies:

```javascript
{...
  "dependencies": {
    "transducers-js": "0.4.113"
  }
 ...}
```

### Bower

You can also include transducers-js in your `bower.json` dependencies:

```javascript
{...
  "dependencies": {
    "transducers-js": "0.4.113"
  }
 ...}
```

## Usage

### Requiring

To import the library under Node.js you can just use `require`:

```js
var t = require("transducers-js");
```

The browser release of the library simply exports a top level
`transducers` object:

```js
var t = transducers;
```

### Basic Usage

With <=ES5:

```js
var map    = t.map,
    filter = t.filter,
    comp   = t.comp,
    into   = t.into;

var inc = function(n) { return n + 1; };
var isEven = function(n) { return n % 2 == 0; };
var xf = comp(map(inc), filter(isEven));

console.log(into([], xf, [0,1,2,3,4])); // [2,4]
```

With ES6:

```js
let {map, filter, comp, into} = t;

let inc = (n) => n + 1;
let isEven = (n) => n % 2 == 0;
let xf = comp(map(inc), filter(isEven));

console.log(into([], xf, [0,1,2,3,4])); // [2,4]
```

## Documentation

Documentation can be found [here](http://cognitect-labs.github.io/transducers-js).

## Integration

transducers-js can also easily be used in combination with *existing*
reduce implementations, whether native or the shims provided by
[Underscore](http://underscorejs.org) and
[Lodash](http://lodash.com). Doing so with native and Underscore can
deliver significant performance benefits. transducers may be easily
converted from their object representation into the necessary
two-arity function via `toFn`.

```js
var arr   = [0,1,2,3,4,5,6,7,8,9,10],
    apush = function(arr, x) { arr.push(x); return arr; },
    xf    = comp(map(inc), filter(isEven));

arr.reduce(toFn(xf, apush), []); // native
_(arr).reduce(toFn(xf, apush), []); // underscore or lodash
```

### Immutable-js

transducers-js can work with custom collection types and still
deliver the same performance benefits, for example with Immutable-js:

```js
var Immutable  = require("immutable"),
    t          = require("transducers-js"),
    comp       = t.comp,
    map        = t.map,
    filter     = t.filter,
    transduce  = t.transduce,

var inc = function(n) { return n + 1; };
var isEven = function(n) { return n % 2 == 0; };
var sum = function(a,b) { return a+b; };

var largeVector = Immutable.Vector();

for(var i = 0; i < 1000000; i++) {
    largeVector = largeVector.push(i);
}

// built in Immutable-js functionality
largeVector.map(inc).filter(isEven).reduce(sum);

// faster with transducers
var xf = comp(map(inc),filter(isEven));
transduce(xf, sum, 0, largeVector);
```

## Building

Fetch the dependecies:

```
bin/deps
```

To build for Node.js

```
bin/build_release_node
```

To build for the browser

```
bin/build_release_browser
```

## Running the tests

Make sure you've first fetched the dependencies, then:

```
bin/test
```

## Contributing 

This library is open source, developed internally by [Cognitect](http://cognitect.com). Issues can be filed using [GitHub Issues](https://github.com/cognitect-labs/transducers-js/issues).

This project is provided without support or guarantee of continued development.
Because transducers-js may be incorporated into products or client projects, we prefer to do development internally and do not accept pull requests or patches. 

## Copyright and License

Copyright © 2014 Cognitect

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
