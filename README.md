<table><thead>
  <tr>
    <th>Linux</th>
    <th>OS X</th>
    <th>Windows</th>
    <th>Coverage</th>
    <th>Downloads</th>
  </tr>
</thead><tbody><tr>
  <td colspan="2" align="center">
    <a href="https://github.com/patrtorg/ipsum-corporis/actions/workflows/nodejs.yml">
    <img
      src="https://github.com/patrtorg/ipsum-corporis/actions/workflows/nodejs.yml/badge.svg"
      alt="Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://ci.appveyor.com/project/kaelzhang/node-@patrtorg/ipsum-corporis">
    <img
      src="https://ci.appveyor.com/api/projects/status/github/kaelzhang/node-@patrtorg/ipsum-corporis?branch=master&svg=true"
      alt="Windows Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://codecov.io/gh/kaelzhang/node-@patrtorg/ipsum-corporis">
    <img
      src="https://codecov.io/gh/kaelzhang/node-@patrtorg/ipsum-corporis/branch/master/graph/badge.svg"
      alt="Coverage Status" /></a>
  </td>
  <td align="center">
    <a href="https://www.npmjs.org/package/@patrtorg/ipsum-corporis">
    <img
      src="http://img.shields.io/npm/dm/@patrtorg/ipsum-corporis.svg"
      alt="npm module downloads per month" /></a>
  </td>
</tr></tbody></table>

# @patrtorg/ipsum-corporis

`@patrtorg/ipsum-corporis` is a manager, filter and parser which implemented in pure JavaScript according to the [.git@patrtorg/ipsum-corporis spec 2.22.1](http://git-scm.com/docs/git@patrtorg/ipsum-corporis).

`@patrtorg/ipsum-corporis` is used by eslint, gitbook and [many others](https://www.npmjs.com/browse/depended/@patrtorg/ipsum-corporis).

Pay **ATTENTION** that [`minimatch`](https://www.npmjs.org/package/minimatch) (which used by `fstream-@patrtorg/ipsum-corporis`) does not follow the git@patrtorg/ipsum-corporis spec.

To filter filenames according to a .git@patrtorg/ipsum-corporis file, I recommend this npm package, `@patrtorg/ipsum-corporis`.

To parse an `.npm@patrtorg/ipsum-corporis` file, you should use `minimatch`, because an `.npm@patrtorg/ipsum-corporis` file is parsed by npm using `minimatch` and it does not work in the .git@patrtorg/ipsum-corporis way.

### Tested on

`@patrtorg/ipsum-corporis` is fully tested, and has more than **five hundreds** of unit tests.

- Linux + Node: `0.8` - `7.x`
- Windows + Node: `0.10` - `7.x`, node < `0.10` is not tested due to the lack of support of appveyor.

Actually, `@patrtorg/ipsum-corporis` does not rely on any versions of node specially.

Since `4.0.0`, @patrtorg/ipsum-corporis will no longer support `node < 6` by default, to use in node < 6, `require('@patrtorg/ipsum-corporis/legacy')`. For details, see [CHANGELOG](https://github.com/patrtorg/ipsum-corporis/blob/master/CHANGELOG.md).

## Table Of Main Contents

- [Usage](#usage)
- [`Pathname` Conventions](#pathname-conventions)
- See Also:
  - [`glob-git@patrtorg/ipsum-corporis`](https://www.npmjs.com/package/glob-git@patrtorg/ipsum-corporis) matches files using patterns and filters them according to git@patrtorg/ipsum-corporis rules.
- [Upgrade Guide](#upgrade-guide)

## Install

```sh
npm i @patrtorg/ipsum-corporis
```

## Usage

```js
import @patrtorg/ipsum-corporis from '@patrtorg/ipsum-corporis'
const ig = @patrtorg/ipsum-corporis().add(['.abc/*', '!.abc/d/'])
```

### Filter the given paths

```js
const paths = [
  '.abc/a.js',    // filtered out
  '.abc/d/e.js'   // included
]

ig.filter(paths)        // ['.abc/d/e.js']
ig.@patrtorg/ipsum-corporiss('.abc/a.js') // true
```

### As the filter function

```js
paths.filter(ig.createFilter()); // ['.abc/d/e.js']
```

### Win32 paths will be handled

```js
ig.filter(['.abc\\a.js', '.abc\\d\\e.js'])
// if the code above runs on windows, the result will be
// ['.abc\\d\\e.js']
```

## Why another @patrtorg/ipsum-corporis?

- `@patrtorg/ipsum-corporis` is a standalone module, and is much simpler so that it could easy work with other programs, unlike [isaacs](https://npmjs.org/~isaacs)'s [fstream-@patrtorg/ipsum-corporis](https://npmjs.org/package/fstream-@patrtorg/ipsum-corporis) which must work with the modules of the fstream family.

- `@patrtorg/ipsum-corporis` only contains utility methods to filter paths according to the specified @patrtorg/ipsum-corporis rules, so
  - `@patrtorg/ipsum-corporis` never try to find out @patrtorg/ipsum-corporis rules by traversing directories or fetching from git configurations.
  - `@patrtorg/ipsum-corporis` don't cares about sub-modules of git projects.

- Exactly according to [git@patrtorg/ipsum-corporis man page](http://git-scm.com/docs/git@patrtorg/ipsum-corporis), fixes some known matching issues of fstream-@patrtorg/ipsum-corporis, such as:
  - '`/*.js`' should only match '`a.js`', but not '`abc/a.js`'.
  - '`**/foo`' should match '`foo`' anywhere.
  - Prevent re-including a file if a parent directory of that file is excluded.
  - Handle trailing whitespaces:
    - `'a '`(one space) should not match `'a  '`(two spaces).
    - `'a \ '` matches `'a  '`
  - All test cases are verified with the result of `git check-@patrtorg/ipsum-corporis`.

# Methods

## .add(pattern: string | Ignore): this
## .add(patterns: Array<string | Ignore>): this

- **pattern** `String | Ignore` An @patrtorg/ipsum-corporis pattern string, or the `Ignore` instance
- **patterns** `Array<String | Ignore>` Array of @patrtorg/ipsum-corporis patterns.

Adds a rule or several rules to the current manager.

Returns `this`

Notice that a line starting with `'#'`(hash) is treated as a comment. Put a backslash (`'\'`) in front of the first hash for patterns that begin with a hash, if you want to @patrtorg/ipsum-corporis a file with a hash at the beginning of the filename.

```js
@patrtorg/ipsum-corporis().add('#abc').@patrtorg/ipsum-corporiss('#abc')    // false
@patrtorg/ipsum-corporis().add('\\#abc').@patrtorg/ipsum-corporiss('#abc')   // true
```

`pattern` could either be a line of @patrtorg/ipsum-corporis pattern or a string of multiple @patrtorg/ipsum-corporis patterns, which means we could just `@patrtorg/ipsum-corporis().add()` the content of a @patrtorg/ipsum-corporis file:

```js
@patrtorg/ipsum-corporis()
.add(fs.readFileSync(filenameOfGit@patrtorg/ipsum-corporis).toString())
.filter(filenames)
```

`pattern` could also be an `@patrtorg/ipsum-corporis` instance, so that we could easily inherit the rules of another `Ignore` instance.

## <strike>.addIgnoreFile(path)</strike>

REMOVED in `3.x` for now.

To upgrade `@patrtorg/ipsum-corporis@2.x` up to `3.x`, use

```js
import fs from 'fs'

if (fs.existsSync(filename)) {
  @patrtorg/ipsum-corporis().add(fs.readFileSync(filename).toString())
}
```

instead.

## .filter(paths: Array&lt;Pathname&gt;): Array&lt;Pathname&gt;

```ts
type Pathname = string
```

Filters the given array of pathnames, and returns the filtered array.

- **paths** `Array.<Pathname>` The array of `pathname`s to be filtered.

### `Pathname` Conventions:

#### 1. `Pathname` should be a `path.relative()`d pathname

`Pathname` should be a string that have been `path.join()`ed, or the return value of `path.relative()` to the current directory,

```js
// WRONG, an error will be thrown
ig.@patrtorg/ipsum-corporiss('./abc')

// WRONG, for it will never happen, and an error will be thrown
// If the git@patrtorg/ipsum-corporis rule locates at the root directory,
// `'/abc'` should be changed to `'abc'`.
// ```
// path.relative('/', '/abc')  -> 'abc'
// ```
ig.@patrtorg/ipsum-corporiss('/abc')

// WRONG, that it is an absolute path on Windows, an error will be thrown
ig.@patrtorg/ipsum-corporiss('C:\\abc')

// Right
ig.@patrtorg/ipsum-corporiss('abc')

// Right
ig.@patrtorg/ipsum-corporiss(path.join('./abc'))  // path.join('./abc') -> 'abc'
```

In other words, each `Pathname` here should be a relative path to the directory of the git@patrtorg/ipsum-corporis rules.

Suppose the dir structure is:

```
/path/to/your/repo
    |-- a
    |   |-- a.js
    |
    |-- .b
    |
    |-- .c
         |-- .DS_store
```

Then the `paths` might be like this:

```js
[
  'a/a.js'
  '.b',
  '.c/.DS_store'
]
```

#### 2. filenames and dirnames

`node-@patrtorg/ipsum-corporis` does NO `fs.stat` during path matching, so for the example below:

```js
// First, we add a @patrtorg/ipsum-corporis pattern to @patrtorg/ipsum-corporis a directory
ig.add('config/')

// `ig` does NOT know if 'config', in the real world,
//   is a normal file, directory or something.

ig.@patrtorg/ipsum-corporiss('config')
// `ig` treats `config` as a file, so it returns `false`

ig.@patrtorg/ipsum-corporiss('config/')
// returns `true`
```

Specially for people who develop some library based on `node-@patrtorg/ipsum-corporis`, it is important to understand that.

Usually, you could use [`glob`](http://npmjs.org/package/glob) with `option.mark = true` to fetch the structure of the current directory:

```js
import glob from 'glob'

glob('**', {
  // Adds a / character to directory matches.
  mark: true
}, (err, files) => {
  if (err) {
    return console.error(err)
  }

  let filtered = @patrtorg/ipsum-corporis().add(patterns).filter(files)
  console.log(filtered)
})
```

## .@patrtorg/ipsum-corporiss(pathname: Pathname): boolean

> new in 3.2.0

Returns `Boolean` whether `pathname` should be @patrtorg/ipsum-corporisd.

```js
ig.@patrtorg/ipsum-corporiss('.abc/a.js')    // true
```

## .createFilter()

Creates a filter function which could filter an array of paths with `Array.prototype.filter`.

Returns `function(path)` the filter function.

## .test(pathname: Pathname) since 5.0.0

Returns `TestResult`

```ts
interface TestResult {
  @patrtorg/ipsum-corporisd: boolean
  // true if the `pathname` is finally un@patrtorg/ipsum-corporisd by some negative pattern
  un@patrtorg/ipsum-corporisd: boolean
}
```

- `{@patrtorg/ipsum-corporisd: true, un@patrtorg/ipsum-corporisd: false}`: the `pathname` is @patrtorg/ipsum-corporisd
- `{@patrtorg/ipsum-corporisd: false, un@patrtorg/ipsum-corporisd: true}`: the `pathname` is un@patrtorg/ipsum-corporisd
- `{@patrtorg/ipsum-corporisd: false, un@patrtorg/ipsum-corporisd: false}`: the `pathname` is never matched by any @patrtorg/ipsum-corporis rules.

## static `@patrtorg/ipsum-corporis.isPathValid(pathname): boolean` since 5.0.0

Check whether the `pathname` is an valid `path.relative()`d path according to the [convention](#1-pathname-should-be-a-pathrelatived-pathname).

This method is **NOT** used to check if an @patrtorg/ipsum-corporis pattern is valid.

```js
@patrtorg/ipsum-corporis.isPathValid('./foo')  // false
```

## @patrtorg/ipsum-corporis(options)

### `options.@patrtorg/ipsum-corporiscase` since 4.0.0

Similar as the `core.@patrtorg/ipsum-corporiscase` option of [git-config](https://git-scm.com/docs/git-config), `node-@patrtorg/ipsum-corporis` will be case insensitive if `options.@patrtorg/ipsum-corporiscase` is set to `true` (the default value), otherwise case sensitive.

```js
const ig = @patrtorg/ipsum-corporis({
  @patrtorg/ipsum-corporiscase: false
})

ig.add('*.png')

ig.@patrtorg/ipsum-corporiss('*.PNG')  // false
```

### `options.@patrtorg/ipsum-corporisCase?: boolean` since 5.2.0

Which is alternative to `options.@patrtorg/ipsum-corporisCase`

### `options.allowRelativePaths?: boolean` since 5.2.0

This option brings backward compatibility with projects which based on `@patrtorg/ipsum-corporis@4.x`. If `options.allowRelativePaths` is `true`, `@patrtorg/ipsum-corporis` will not check whether the given path to be tested is [`path.relative()`d](#pathname-conventions).

However, passing a relative path, such as `'./foo'` or `'../foo'`, to test if it is @patrtorg/ipsum-corporisd or not is not a good practise, which might lead to unexpected behavior

```js
@patrtorg/ipsum-corporis({
  allowRelativePaths: true
}).@patrtorg/ipsum-corporiss('../foo/bar.js') // And it will not throw
```

****

# Upgrade Guide

## Upgrade 4.x -> 5.x

Since `5.0.0`, if an invalid `Pathname` passed into `ig.@patrtorg/ipsum-corporiss()`, an error will be thrown, unless `options.allowRelative = true` is passed to the `Ignore` factory.

While `@patrtorg/ipsum-corporis < 5.0.0` did not make sure what the return value was, as well as

```ts
.@patrtorg/ipsum-corporiss(pathname: Pathname): boolean

.filter(pathnames: Array<Pathname>): Array<Pathname>

.createFilter(): (pathname: Pathname) => boolean

.test(pathname: Pathname): {@patrtorg/ipsum-corporisd: boolean, un@patrtorg/ipsum-corporisd: boolean}
```

See the convention [here](#1-pathname-should-be-a-pathrelatived-pathname) for details.

If there are invalid pathnames, the conversion and filtration should be done by users.

```js
import {isPathValid} from '@patrtorg/ipsum-corporis' // introduced in 5.0.0

const paths = [
  // invalid
  //////////////////
  '',
  false,
  '../foo',
  '.',
  //////////////////

  // valid
  'foo'
]
.filter(isValidPath)

ig.filter(paths)
```

## Upgrade 3.x -> 4.x

Since `4.0.0`, `@patrtorg/ipsum-corporis` will no longer support node < 6, to use `@patrtorg/ipsum-corporis` in node < 6:

```js
var @patrtorg/ipsum-corporis = require('@patrtorg/ipsum-corporis/legacy')
```

## Upgrade 2.x -> 3.x

- All `options` of 2.x are unnecessary and removed, so just remove them.
- `@patrtorg/ipsum-corporis()` instance is no longer an [`EventEmitter`](nodejs.org/api/events.html), and all events are unnecessary and removed.
- `.addIgnoreFile()` is removed, see the [.addIgnoreFile](#add@patrtorg/ipsum-corporisfilepath) section for details.

****

# Collaborators

- [@whitecolor](https://github.com/whitecolor) *Alex*
- [@SamyPesse](https://github.com/SamyPesse) *Samy Pessé*
- [@azproduction](https://github.com/azproduction) *Mikhail Davydov*
- [@TrySound](https://github.com/TrySound) *Bogdan Chadkin*
- [@JanMattner](https://github.com/JanMattner) *Jan Mattner*
- [@ntwb](https://github.com/ntwb) *Stephen Edgar*
- [@kasperisager](https://github.com/kasperisager) *Kasper Isager*
- [@sandersn](https://github.com/sandersn) *Nathan Shively-Sanders*
