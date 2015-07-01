# Coffee-React

Coffee-React provides a JSX-like syntax for building [React](http://facebook.github.io/react/) components with the full awesomeness of CoffeeScript.  [Try it out](https://jsdf.github.io/coffee-react-transform/).

Included is the `cjsx` executable, which is wrapper for `coffee`, using
[coffee-react-transform](https://github.com/jsdf/coffee-react-transform) and
[coffee-script](https://github.com/jashkenas/coffeescript) to transform CJSX to Javascript.
You can also `require()` CJSX components under [node](http://nodejs.org) for server-side rendering.

### Example

neat-component.cjsx
```coffee
NeatComponent = React.createClass
  render: ->
    <div className="neat-component">
      {<h1>A Component is I</h1> if @props.showTitle}
      <hr />
      {<p key={n}>This line has been printed {n} times</p> for n in [1..5]}
    </div>
```

compile it
```bash
$ cjsx -cb neat-component.cjsx
```

neat-component.js
```js
// Generated by CoffeeScript 1.9.1
var NeatComponent;

NeatComponent = React.createClass({displayName: "NeatComponent",
  render: function() {
    var n;
    return React.createElement("div", {
      "className": "neat-component"
    }, (this.props.showTitle ? React.createElement("h1", null, "A Component is I") : void 0), React.createElement("hr", null), (function() {
      var i, results;
      results = [];
      for (n = i = 1; i <= 5; n = ++i) {
        results.push(React.createElement("p", {
          "key": n
        }, "This line has been printed ", n, " times"));
      }
      return results;
    })());
  }
});
```

### Installation
```bash
npm install -g coffee-react
```

#### Version compatibility
- 3.x - React 0.13.x
- 2.1.x - React 0.12.1
- 2.x - React 0.12
- 1.x - React 0.11.2
- 0.x - React 0.11 and below

### Usage

```
$ cjsx -h

Usage: cjsx [options] path/to/script.cjsx -- [args]

If called without options, `cjsx` will run your script.

  -b, --bare         compile without a top-level function wrapper
  -c, --compile      compile to JavaScript and save as .js files
  -e, --eval         pass a string from the command line as input
  -h, --help         display this help message
  -j, --join         concatenate the source CoffeeScript before compiling
  -m, --map          generate source map and save as .map files
  -n, --nodes        print out the parse tree that the parser produces
      --nodejs       pass options directly to the "node" binary
      --no-header    suppress the "Generated by" header
  -o, --output       set the output directory for compiled JavaScript
  -p, --print        print out the compiled JavaScript
  -s, --stdio        listen for and compile scripts over stdio
  -l, --literate     treat stdio as literate style coffee-script
  -t, --tokens       print out the tokens that the lexer/rewriter produce
  -v, --version      display the version number
  -w, --watch        watch scripts for changes and rerun commands

```

Output compiled JS to a file of the same name:
```bash
$ cjsx -c my-component.cjsx
```

#### Require .cjsx files under node
As with the `coffee-script` module, you need to register `.cjsx` with the module loader:
```coffee
require('coffee-react/register')

Component = require('./component.cjsx')

```

### Spread attributes
A recent addition to JSX (and CJSX) is 'spread attributes' which allow merging an object of props into a component, eg:
```coffee
extraProps = color: 'red', speed: 'fast'
<div color="blue" {... extraProps} />
```
which is transformed to:
```coffee
extraProps = color: 'red', speed: 'fast'
React.createElement(React.DOM.div, React.__spread({"color": "blue"}, extraProps)
```

### Breaking Changes in 1.0

React 0.12 will introduce changes to the way component descriptors are constructed, where the return value of `React.createClass` is not a descriptor factory but simply the component class itself, and descriptors must be created manually using `React.createElement` or by wrapping the component class with `React.createDescriptor`.

In preparation for this, coffee-react-transform (and as a result, coffee-react) now outputs calls to `React.createElement` to construct element descriptors from component classes for you, so you won't need to [wrap your classes using `React.createFactory`](https://gist.github.com/sebmarkbage/ae327f2eda03bf165261). However, for this to work you will need to be using at least React 0.11.2, which adds `React.createElement`.

If you want the older style JSX output (which just desugars into function calls) then you need to use the 0.x branch, eg. 0.5.1.

Additionally, as of 1.0.0, all input files will be CJSX transformed, even if they don't have a `.cjsx` extension or `# @cjsx` pragma.

### Related projects
- [coffee-react-transform](https://github.com/jsdf/coffee-react-transform), the underlying parser/transformer package.
- [node-cjsx](https://github.com/SimonDegraeve/node-cjsx): `require` CJSX files on the server (also possible with [coffee-react/register](https://github.com/jsdf/coffee-react)).
- [coffee-reactify](https://github.com/jsdf/coffee-reactify): bundle CJSX files via [browserify](https://github.com/substack/node-browserify), see also [cjsxify](https://github.com/SimonDegraeve/cjsxify).  
- [react-coffee-quickstart](https://github.com/SimonDegraeve/react-coffee-quickstart): equivalent to [react-quickstart](https://github.com/andreypopp/react-quickstart).
- [sprockets preprocessor](https://github.com/jsdf/sprockets-coffee-react): use CJSX with Rails/Sprockets
- [ruby coffee-react gem](https://github.com/jsdf/ruby-coffee-react): transform CJSX to Coffeescript under Ruby
- [vim plugin](https://github.com/mtscout6/vim-cjsx) for syntax highlighting
- [sublime text package](https://github.com/Guidebook/sublime-cjsx) for syntax highlighting
- [mimosa plugin](https://github.com/mtscout6/mimosa-cjsx) for the mimosa build tool
- [gulp plugin](https://github.com/mtscout6/gulp-cjsx) for the gulp build tool
- [karma preprocessor](https://github.com/mtscout6/karma-cjsx-preprocessor) for karma test runner
