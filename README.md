[![build status](https://secure.travis-ci.org/mjackson/bufferedstream.png)](http://travis-ci.org/mjackson/bufferedstream)

BufferedStream is a reliable read/write stream class for node.js and browsers. All data that is written to a BufferedStream is buffered until the next turn of the event loop. This greatly enhances the usability of streams by making it easy to setup listeners in the same turn of the event loop before data is emitted.

This class follows the first version of the node streams API, which is powerful because of its simplicity. Node has since moved on to other, much more complex streams implementations, but there never was a problem with the initial API. The only problems were with node's implementation. For example, streams did not always wait until the next tick to emit data. Also, some streams did not respect `pause`/`resume` semantics.

BufferedStream addresses these problems by providing a well-tested, performant implementation that preserves the original streams API and works in both node.js and browsers.

## Installation

Using [npm](http://npmjs.org):

    $ npm install bufferedstream

## Usage

The key feature of this class is that anything you write to the stream in the current turn of the event loop is buffered until the next one. This allows you to register event handlers, pause the stream, etc. reliably without losing any data.

```javascript
var BufferedStream = require('bufferedstream');

var stream = new BufferedStream;
stream.write('Hello ');
stream.pause();

setTimeout(function () {
  stream.write('IHdvcmxkLg==', 'base64');
  stream.resume();
  stream.on('data', function (chunk) {
    console.log(chunk.toString()); // Hello world.
  });
}, 10);
```

The `BufferedStream` constructor may also accept a "source" which may be another stream that will be piped directly through to this stream or a string. This is useful for wrapping various stream-like objects and normalizing their behavior across implementations.

Please see the source code for more information. The module is small enough (and well-documented) that it should be easy to digest in a quick skim.

## Specs

Run the specs with [mocha](http://visionmedia.github.com/mocha/):

    $ mocha spec

## License

Copyright 2014 Michael Jackson

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or the use or other dealings in the software.
