# Best

**Best** is a **b**are t**est** runner. It looks for test sources, finds exported functions,
and runs them - without requiring any imports or exposing any globals.

All functions are awaited upon, but run in series.

## Installation

Best can be installed with either yarn or npm.

```console
$ yarn add --dev @zeit/best
$ npm install --save-dev @zeit/best
```

## Usage

```
  best

  A dead simple test runner.

  - Export your functions and they're evaluated
  - Function names become test names
  - Exported functions are awaited upon

  USAGE

    best --help
    best [-I /dir/or/file [-I ...]] [-T] [test_name...]

    test_names's correspond to the name of the exported functions within test sources,
    and are prefixed with the path of the source file (sans extension) in which it was defined.

    For example, the following test function inside test/foo.js:

        exports.my_example_test = async () => {
            assert(foo === bar);
        };

    would translate to the test_name:

        test/foo/my_example_test

    Specify one or more (optional) test_name's to only run certain tests (or prefix
    with - to skip the named test).

  OPTIONS

    --help                          Shows this help message

    -v, --verbose                   Shows more verbose test results

    -I, --include /dir/or/file      Uses one or more directories/files as test sources.
                                    Defaults to ./test/**/*.js if no include directives
                                    are specified

    -r, --require module-name       Imports a module or a script prior to running tests

```

### Typescript

To use with Typescript, install `ts-node` and `esm` and register them.

```console
$ npm install --save-dev ts-node esm
$ best -r esm -r ts-node
```

> **NOTE:** `esm` must come before `ts-node`!

### Debugging Hangs

Best does not have a built-in timeout for tests in order to remain as unopinionated as possible.
Due to this, hangs may occur from time to time.

Using `why-is-node-running`, we can detect hangs and see what caused them:

```console
$ npm install --save-dev why-is-node-running
$ best -r why-is-node-running/include
...
$ pkill -USR1 node
```

Sending `SIGUSR1` to the process will cause `why-is-node-running` to dump statistics
about remaining (ref'd) handles to the TTY of the node process itself.

## License
Best is copyright &copy; 2018-2019 by ZEIT, Inc. and released under the [MIT License](LICENSE).
