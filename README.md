[![Build Status](https://travis-ci.org/angular/angular.svg?branch=master)](https://travis-ci.org/angular/angular)

## Build

### Prerequisites:

1. `npm install`
2. `npm install -g gulp karma karma-cli`
3. [Install the Dart SDK](https://www.dartlang.org/tools/sdk/)
4. [Add the Dart SDK's `bin` directory to your system path](https://www.dartlang.org/tools/pub/installing.html)
5. `gulp build`
6. `pub get`

### Folder structure

* `modules/*`: modules that will be loaded in the browser
* `tools/*`: tools that are needed to build Angular

### File endings

* `*.js`: javascript files that get transpiled to Dart and EcmaScript 5
* `*.es6`: javascript files that get transpiled only to EcmaScript 5
* `*.es5`: javascript files that don't get transpiled
* `*.dart`: dart files that don't get transpiled

### Build:

1. `gulp build` -> result is in `build` folder

  * will also run `pub get` for the subfolders in `modules`
    and run `dartanalyzer` for every file that matches
    `<module>/src/<module>.dart`, e.g. `di/src/di.dart`

2. `gulp clean` -> cleans the `build` folder

### Tests:

1. `karma start karma-js.conf.js`: JS tests
2. `karma start karma-dart.conf.js`: Dart tests

Notes for all tests:

The karma preprocessor is setup in a way so that after every test run
the transpiler is reloaded. With that it is possible to make changes
to the preprocessor and run the tests without exiting karma
(just touch a test file that you would like to run).

Restriction for Dart tests (for now):

  * Due to a bug `karma-dart` plugin,
    this will use the files in the `build` folder for resolving
    `package:` dependencies (created e.g. for `import ... from 'di:di'`).
    So you need to execute `gulp build` before this.

## Debug the transpiler

If you need to debug the transpiler:

- add a `debugger;` statement in the transpiler code,
- from the root folder, execute `node debug node_modules/.bin/gulp build` to enter the node
  debugger
- press "c" to execute the program until you reach the `debugger;` statement,
- you can then type "repl" to enter the REPL and inspect variables in the context.

See the [Node.js manual](http://nodejs.org/api/debugger.html) for more information.

Notes:
- You can also execute `node node_modules/.bin/karma start karma-dart.conf.js` depending on which
  code you want to debug (the former will process the "modules" folder while the later processes
  the transpiler specs),
- You can also add `debugger;` statements in the specs (JavaScript). The execution will halt when
  the developer tools are opened in the browser running Karma.
