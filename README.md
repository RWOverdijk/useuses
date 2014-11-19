# Useuses
A module that allows you to build your project, resolving dependencies based on the `@uses` annotation.

## What?
Before I dive into the technical specifics, I'll explain what this module is all about.

In short, it allows you to annotate your files with the `@uses` annotation to specify your dependencies;
which is convenient for the developer reading your code as he or she now knows what dependencies a file has.
It looks like this:

```javascript
/**
 * My file
 *
 * Some info about My file
 *
 * @author  RWOverdijk
 * @version 0.1.0
 * @license MIT
 *
 * @uses ./my-dependency.js
 * @uses ./my/other/dependency.js
 */
 // Code here...
```

It's also convenient because this module will bundle all dependencies together for you.
If you'd like a more detailed explanation of this module and its benefits, you can read about it in this [blog post](http://blog.spoonx.nl/javascript-dependency-management/).

## Installation
You can install useuses using npm:

**Save as dependency:**
`npm install useuses --save`

**Global (for the cli):**
`npm install -g useuses`

## Usage
This module can be used in a programmatic manner, or via the command line.

### CLI
This example assumes you have useuses installed globally.
If that's not the case, simply replace `useuses` with `./node_modules/useuses/bin/useuses.js`.

`useuses -i example/main.js -o example/dist/built.js -w`

### Programmatic
Below is a full example of how to use Useuses.

```javascript
var Useuses = require('useuses'),
    useuses,
    options;

options = {
  in  : 'example/main.js',
  out : 'example/dist/built.js',
  wrap: true
};

useuses = new Useuses(options);

useuses.compile(function (error, assembled) {
  if (error) {
    return console.error(error);
  }

  console.log('Yay! The build succeeded. The files included in the build are:', assembled);
});
```

## Options
The following options are currently available for useuses.

### In (--in, -i)
Use this option to tel useuses where the main project file is located.

### Out (--out, -o)
Using this option you can tell useuses where to write the built file to.

### Wrap (--wrap, -w)
Setting this to true, will instruct useuses to wrap the built code in a self-invoking function.
The advantage here, is that your code will not pollute the global scope; but will still run.

For example, this:

```javascript
var name = 'World';

console.log('Hello ' + name);
```

Would become this:

```javascript
(function () {
  var name = 'World';

  console.log('Hello ' + name);
})();
```
