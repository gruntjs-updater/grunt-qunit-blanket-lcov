# grunt-qunit-blanket-lcov [![npm version](https://badge.fury.io/js/grunt-qunit-blanket-lcov.svg)](http://badge.fury.io/js/grunt-qunit-blanket-lcov)

> Save LCOV reports generated by Blanket for Grunt QUnit.

This is used to save [Blanket.js](http://blanketjs.org) tests coverage results done with [grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit) in order to send them to Coveralls.io with [grunt-coveralls](https://github.com/pimterry/grunt-coveralls).

## Getting Started

### Setup the task
If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-qunit-blanket-lcov --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-qunit-blanket-lcov');
```

### Setup Blanket.js
Simply follow Blanket.js guide to setup the test coverage with QUnit. No special configuration of the test suite is needed.

Your test HTML should look like :

```html
<!DOCTYPE html>
<html>
<head>
  <title>My test suite</title>
  <meta charset="utf-8">
  
  <link rel="stylesheet" href="../bower_components/qunit/qunit/qunit.css">
  
  <script src="../bower_components/jquery/dist/jquery.min.js"></script>
  <script src="../bower_components/qunit/qunit/qunit.js"></script>
  <script src="../bower_components/blanket/dist/qunit/blanket.min.js"></script>
  
  <script src="../src/library.js" data-cover></script>

  <script src="core.js"></script>
</head>

<body>
  <!-- -->
</body>
</html>
```

Don't forget to change the `urls` of the `qunit` task to add `?coverage=true` for each test suite.

## The "qunit_blanket_lcov" task

### Overview
In your project's Gruntfile, add a section named `qunit_blanket_lcov` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  qunit_blanket_lcov: {
    options: {
      inject_reporter: true
    },
    lib: {
      src: 'src/library.js',
      dest: '.coverage-results/library.lcov'
    }
  }
});
```

Then add or edit your `test` Grunt task to execute `qunit_blanket_lcov` **before** `qunit`.

```js
grunt.registerTask('test', [
    'qunit_blanket_lcov',
    'qunit'
]);
```

### Options

#### inject_reporter
Type: `boolean`
Default value: `true`

Automatically insert the custom Blanket reporter in the PhantomJS instance. If you redefined the `inject` option of `grunt-contrib-qunit` you must set this option to `false` and manually add `node_modules/grunt-qunit-blanket-lcov/reporter.js` to the inject list.

### Usage Examples

#### One file
In this example the test coverage results of `library.js` will be saved to `library.lcov`.

```js
grunt.initConfig({
  qunit_blanket_lcov: {
    lib: {
      src: 'src/library.js',
      dest: '.coverage-results/library.lcov'
    }
  }
});
```

#### Multiple files
If you have many source files used by QUnit analyzed by Blanket it might be a good idea to use the Grunt files globbing option. This example will save all coverage results with renaming for some files.

```js
grunt.initConfig({
  qunit_blanket_lcov: {
    all: {
        files: [{
            expand: true,
            src: ['src/*.js', 'src/plugins/**/plugin.js'],
            dest: '.coverage-results',
            ext: '.lcov',
            rename: function(dest, src) {
                if (src.indexOf('plugins') !== -1) {
                    src = src.replace(/plugins\/([^\/]+)\/plugin.lcov$/, 'plugin-$1.lcov');
                }
                return dest + src.replace(/^src/, '');
            }
        }]
    }
  }
});
```
