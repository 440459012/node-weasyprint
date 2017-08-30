node-weasyprint [![Build Status](https://travis-ci.org/devongovett/node-weasyprint.svg)](https://travis-ci.org/devongovett/node-weasyprint)
================

A Node.js wrapper for the [weasyprint](http://weasyprint.org/) command line tool.  As the name implies, 
it converts HTML documents to PDFs using WebKit.

## Installation

First, you need to install the `weasyprint` command line tool on your system.

The **easiest way** to do this is to
[download](http://weasyprint.org/downloads.html#stable) a prebuilt version for your system.  **DO NOT** try to use
the packages provided by your distribution as they may not be using a patched Qt and have missing features.

Finally, to install the node module, use `npm`:

    npm install weasyprint
    
Be sure the `weasyprint` command line tool is in your PATH when you're done installing.  If you don't want to do this for some reason, you can change
the `require('weasyprint').command` property to the path to the `weasyprint` command line tool.

## Usage

### weasyprint(source, [options], [callback]);

```javascript
var weasyprint = require('weasyprint');

// URL
weasyprint('http://google.com/', { pageSize: 'letter' })
  .pipe(fs.createWriteStream('out.pdf'));
  
// HTML
weasyprint('<h1>Test</h1><p>Hello world</p>')
  .pipe(res);

// Stream input and output
var stream = weasyprint(fs.createReadStream('file.html'));

// output to a file directly
weasyprint('http://apple.com/', { output: 'out.pdf' });

// Optional callback
weasyprint('http://google.com/', { pageSize: 'letter' }, function (err, stream) {
  // do whatever with the stream
});

// Repeatable options
weasyprint('http://google.com/', {
  allow : ['path1', 'path2'],
  customHeader : [
    ['name1', 'value1'],
    ['name2', 'value2']
  ]
});

// Ignore warning strings
weasyprint('http://apple.com/', { 
  output: 'out.pdf',
  ignore: ['QFont::setPixelSize: Pixel size <= 0 (0)']
});
// RegExp also acceptable
weasyprint('http://apple.com/', { 
  output: 'out.pdf',
  ignore: [/QFont::setPixelSize/]
});
```

`weasyprint` is just a function, which you call with either a URL or an inline HTML string, and it returns
a stream that you can read from or pipe to wherever you like (e.g. a file, or an HTTP response).

## Options

There are [many options](http://weasyprint.org/docs.html) available to
weasyprint.  All of the command line options are supported as documented on the page linked to above.  The
options are camelCased instead-of-dashed as in the command line tool. Note that options that do not have values, must be specified as a boolean, e.g. **debugJavascript: true**

There is also an `output` option that can be used to write the output directly to a filename, instead of returning
a stream.

### Debug Options

Apart from the **debugJavascript** option from weasyprint, there is an additional options **debug** and **debugStdOut** which will help you debug rendering issues, by outputting data to the console. **debug** prints and **stderr** messages while **debugStdOut** prints any **stdout** warning messages.

## Tests

Run `npm test` and manually check that generated files are like the expected files. The test suit prints the paths of the files that needs to be compared.

**TODO** - Find a way to automatically compare the PDF files.

## License

MIT
