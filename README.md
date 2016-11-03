# micromatch [![NPM version](https://img.shields.io/npm/v/micromatch.svg?style=flat)](https://www.npmjs.com/package/micromatch) [![NPM monthly downloads](https://img.shields.io/npm/dm/micromatch.svg?style=flat)](https://npmjs.org/package/micromatch)  [![NPM total downloads](https://img.shields.io/npm/dt/micromatch.svg?style=flat)](https://npmjs.org/package/micromatch) [![Linux Build Status](https://img.shields.io/travis/jonschlinkert/micromatch.svg?style=flat&label=Travis)](https://travis-ci.org/jonschlinkert/micromatch) [![Windows Build Status](https://img.shields.io/appveyor/ci/jonschlinkert/micromatch.svg?style=flat&label=AppVeyor)](https://ci.appveyor.com/project/jonschlinkert/micromatch)

> Glob matching for javascript/node.js. A drop-in replacement and faster alternative to minimatch and multimatch.

## Table of Contents

- [Install](#install)
- [Quickstart](#quickstart)
- [Why use micromatch?](#why-use-micromatch)
  * [Features](#features)
- [Switching from minimatch](#switching-from-minimatch)
- [Switching from multimatch](#switching-from-multimatch)
- [API](#api)
- [Options](#options)
  * [options.dot](#optionsdot)
  * [options.matchBase](#optionsmatchbase)
  * [options.nobrace](#optionsnobrace)
  * [options.nodupes](#optionsnodupes)
  * [options.nocase](#optionsnocase)
  * [options.nonegate](#optionsnonegate)
  * [options.nonull](#optionsnonull)
  * [options.unixify](#optionsunixify)
  * [options.cache](#optionscache)
- [Notes](#notes)
- [Benchmarks](#benchmarks)
- [About](#about)
  * [Related projects](#related-projects)
  * [Contributing](#contributing)
  * [Contributors](#contributors)
  * [Release history](#release-history)
  * [Building docs](#building-docs)
  * [Running tests](#running-tests)
  * [Author](#author)
  * [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save micromatch
```

## Quickstart

```js
var mm = require('micromatch');
mm(list, patterns[, options]);
```

The [main export](#micromatch) takes a list of strings and one or more glob patterns:

```js
console.log(mm(['foo', 'bar', 'qux'], ['f*', 'b*'])); // ['foo', 'bar']
```

Use [.isMatch](#ismatch) to get true/false:

```js
console.log(mm.isMatch('foo', 'f*'));  // true
```

**Switching from [minimatch](#switching-from-minimatch) and [multimatch](#switching-from-minimatch) is easy**:

* [mm()](#usage) is the same as [multimatch()](https://github.com/sindresorhus/multimatch)
* [mm.match()](#match) is the same as [minimatch.match()](https://github.com/isaacs/minimatch)
* use [mm.isMatch()](#ismatch) instead of [minimatch()](https://github.com/isaacs/minimatch)

## Why use micromatch?

> micromatch is a [drop-in replacement](#switch-from-minimatch) for minimatch and multimatch

Micromatch is [safer](https://github.com/jonschlinkert/braces#braces-is-safe), [faster](#benchmarks), more accurate, and supports all of the same matching features as [minimatch](https://github.com/isaacs/minimatch) and [multimatch](https://github.com/sindresorhus/multimatch).

Moreover, micromatch has:

* Better support for the Bash 4.3 specification than minimatch and multimatch
* More than 7,200 [unit tests](./test), with thousands more patterns tested (minimatch and multimatch fail many of the tests)
* Better windows support than minimatch and multimatch

### Features

* Native support for multiple glob patterns (no need for wrappers like multimatch)
* Glob pattern support (`**/*`, `a/b/*.js`, or `['a/*.js', '!b.js']`)
* [extglob](https://github.com/jonschlinkert/extglob) support (`+(x|y)`, `!(a|b)`, etc)
* [POSIX character class](https://github.com/jonschlinkert/expand-brackets) support (`**/[[:alpha:][:digit:]]/`)
* [brace expansion](https://github.com/jonschlinkert/braces) support (`a/b-{1..5}.md`, `one/{two,three}/four.md`)
* regex character classes (`a/b/baz-[1-5].js`)
* regex logical or (`a/b/(abc|xyz).js`)

You can mix and match these features to create whatever patterns you need!

**Example**

```js
mm(['a/b.js', 'a/b.md'], 'a/*.!(js)');
//=> ['a/b.md']
```

## Switching from minimatch

Use `mm.isMatch()` instead of `minimatch()`:

```js
mm.isMatch('foo', 'b*');
//=> false
```

Use `mm.match()` instead of `minimatch.match()`:

```js
mm.match(['foo', 'bar'], 'b*');
//=> 'bar'
```

## Switching from multimatch

Same signature:

```js
mm(['foo', 'bar', 'baz'], ['f*', '*z']);
//=> ['foo', 'baz']
```

## API

### [micromatch](index.js#L39)

The main function takes a list of strings and one or more glob patterns to use for matching.

**Example**

```js
var mm = require('micromatch');
mm(list, patterns[, options]);

console.log(mm(['a.js', 'a.txt'], ['*.js']));
//=> [ 'a.js' ]
```

**Params**

* `list` **{Array}**: A list of strings to match
* `patterns` **{String|Array}**: One or more glob patterns to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Array}**: Returns an array of matches

### [.match](index.js#L107)

Similar to the main function, but `pattern` must be a string.

**Example**

```js
var mm = require('micromatch');
mm.match(list, pattern[, options]);

console.log(mm.match(['a.a', 'a.aa', 'a.b', 'a.c'], '*.a'));
//=> ['a.a', 'a.aa']
```

**Params**

* `list` **{Array}**: Array of strings to match
* `pattern` **{String}**: Glob pattern to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Array}**: Returns an array of matches

### [.isMatch](index.js#L171)

Returns true if the specified `string` matches the given glob `pattern`.

**Example**

```js
var mm = require('micromatch');
mm.isMatch(string, pattern[, options]);

console.log(mm.isMatch('a.a', '*.a'));
//=> true
console.log(mm.isMatch('a.b', '*.a'));
//=> false
```

**Params**

* `string` **{String}**: String to match
* `pattern` **{String}**: Glob pattern to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Boolean}**: Returns true if the string matches the glob pattern.

### [.not](index.js#L201)

Returns a list of strings that _DO NOT MATCH_ any of the given `patterns`.

**Example**

```js
var mm = require('micromatch');
mm.not(list, patterns[, options]);

console.log(mm.not(['a.a', 'b.b', 'c.c'], '*.a'));
//=> ['b.b', 'c.c']
```

**Params**

* `list` **{Array}**: Array of strings to match.
* `patterns` **{String|Array}**: One or more glob pattern to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Array}**: Returns an array of strings that **do not match** the given patterns.

### [.any](index.js#L238)

Returns true if the given `string` matches any of the given glob `patterns`.

**Example**

```js
var mm = require('micromatch');
mm.any(string, patterns[, options]);

console.log(mm.any('a.a', ['b.*', '*.a']));
//=> true
console.log(mm.any('a.a', 'b.*'));
//=> false
```

**Params**

* `str` **{String}**: The string to test.
* `patterns` **{String|Array}**: One or more glob patterns to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Boolean}**: Returns true if any patterns match `str`

### [.contains](index.js#L268)

Returns true if the given `string` contains the given pattern. Similar to [.isMatch](#isMatch) but the pattern can match any part of the string.

**Example**

```js
var mm = require('micromatch');
mm.contains(string, pattern[, options]);

console.log(mm.contains('aa/bb/cc', '*b'));
//=> true
console.log(mm.contains('aa/bb/cc', '*d'));
//=> false
```

**Params**

* `str` **{String}**: The string to match.
* `pattern` **{String}**: Glob pattern to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Boolean}**: Returns true if the patter matches any part of `str`.

### [.matchKeys](index.js#L317)

Filter the keys of the given object with the given `glob` pattern and `options`. Does not attempt to match nested keys. If you need this feature, use [glob-object](https://github.com/jonschlinkert/glob-object) instead.

**Example**

```js
var mm = require('micromatch');
mm.matchKeys(object, patterns[, options]);

var obj = { aa: 'a', ab: 'b', ac: 'c' };
console.log(mm.matchKeys(obj, '*b'));
//=> { ab: 'b' }
```

**Params**

* `object` **{Object}**: The object with keys to filter.
* `patterns` **{String|Array}**: One or more glob patterns to use for matching.
* `options` **{Object}**: Any [options](#options) to change how matches are performed
* `returns` **{Object}**: Returns an object with only keys that match the given patterns.

### [.matcher](index.js#L346)

Returns a memoized matcher function from the given glob `pattern` and `options`. The returned function takes a string to match as its only argument and returns true if the string is a match.

**Example**

```js
var mm = require('micromatch');
mm.matcher(pattern[, options]);

var isMatch = mm.matcher('*.!(*a)');
console.log(isMatch('a.a'));
//=> false
console.log(isMatch('a.b'));
//=> true
```

**Params**

* `pattern` **{String}**: Glob pattern
* `options` **{Object}**: Any [options](#options) to change how matches are performed.
* `returns` **{Function}**: Returns a matcher function.

### [.makeRe](index.js#L403)

Create a regular expression from the given glob `pattern`.

**Example**

```js
var mm = require('micromatch');
mm.makeRe(pattern[, options]);

console.log(mm.makeRe('*.js'));
//=> /^(?:(\.[\\\/])?(?!\.)(?=.)[^\/]*?\.js)$/
```

**Params**

* `pattern` **{String}**: A glob pattern to convert to regex.
* `options` **{Object}**: Any [options](#options) to change how matches are performed.
* `returns` **{RegExp}**: Returns a regex created from the given pattern.

### [.braces](index.js#L446)

Expand the given brace `pattern`.

**Example**

```js
var micromatch = require('micromatch');
console.log(micromatch.braces('foo/{a,b}/bar'));
//=> ['foo/(a|b)/bar']

console.log(micromatch.braces('foo/{a,b}/bar', {expand: true}));
//=> ['foo/(a|b)/bar']
```

**Params**

* `pattern` **{String}**: String with brace pattern to expand.
* `options` **{Object}**: Any [options](#options) to change how expansion is performed. See the [braces](https://github.com/jonschlinkert/braces) library for all available options.
* `returns` **{Array}**

### [.create](index.js#L499)

Parses the given glob `pattern` and returns an object with the compiled `output` and optional source `map`.

**Example**

```js
var mm = require('micromatch');
mm.create(pattern[, options]);

console.log(mm.create('abc/*.js'));
// { options: { source: 'string', sourcemap: true },
//   state: {},
//   compilers:
//    { ... },
//   output: '(\\.[\\\\\\/])?abc\\/(?!\\.)(?=.)[^\\/]*?\\.js',
//   ast:
//    { type: 'root',
//      errors: [],
//      nodes:
//       [ ... ],
//      dot: false,
//      input: 'abc/*.js' },
//   parsingErrors: [],
//   map:
//    { version: 3,
//      sources: [ 'string' ],
//      names: [],
//      mappings: 'AAAA,GAAG,EAAC,kBAAC,EAAC,EAAE',
//      sourcesContent: [ 'abc/*.js' ] },
//   position: { line: 1, column: 28 },
//   content: {},
//   files: {},
//   idx: 6 }
```

**Params**

* `pattern` **{String}**: Glob pattern to parse and compile.
* `options` **{Object}**: Any [options](#options) to change how parsing and compiling is performed.
* `returns` **{Object}**: Returns an object with the parsed AST, compiled string and optional source map.

### [.parse](index.js#L556)

Parse the given `str` with the given `options`.

**Example**

```js
var mm = require('micromatch');
mm.parse(pattern[, options]);

var ast = mm.parse('a/{b,c}/d');
console.log(ast);
// { type: 'root',
//   errors: [],
//   input: 'a/{b,c}/d',
//   nodes:
//    [ { type: 'bos', val: '' },
//      { type: 'text', val: 'a/' },
//      { type: 'brace',
//        nodes:
//         [ { type: 'brace.open', val: '{' },
//           { type: 'text', val: 'b,c' },
//           { type: 'brace.close', val: '}' } ] },
//      { type: 'text', val: '/d' },
//      { type: 'eos', val: '' } ] }
```

**Params**

* `str` **{String}**
* `options` **{Object}**
* `returns` **{Object}**: Returns an AST

### [.compile](index.js#L609)

Compile the given `ast` or string with the given `options`.

**Example**

```js
var micromatch = require('micromatch');
mm.compile(ast[, options]);

var ast = micromatch.parse('a/{b,c}/d');
console.log(micromatch.compile(ast));
// { options: { source: 'string' },
//   state: {},
//   compilers:
//    { eos: [Function],
//      noop: [Function],
//      bos: [Function],
//      brace: [Function],
//      'brace.open': [Function],
//      text: [Function],
//      'brace.close': [Function] },
//   output: [ 'a/(b|c)/d' ],
//   ast:
//    { ... },
//   parsingErrors: [] }
```

**Params**

* `ast` **{Object|String}**
* `options` **{Object}**
* `returns` **{Object}**: Returns an object that has an `output` property with the compiled string.

## Options

### options.dot

Match dotfiles. Same behavior as [minimatch](https://github.com/isaacs/minimatch).

Type: `Boolean`

Default: `false`

### options.matchBase

Allow glob patterns without slashes to match a file path based on its basename. . Same behavior as [minimatch](https://github.com/isaacs/minimatch).

Type: `Boolean`

Default: `false`

**Example**

```js
mm(['a/b.js', 'a/c.md'], '*.js');
//=> []

mm(['a/b.js', 'a/c.md'], '*.js', {matchBase: true});
//=> ['a/b.js']
```

### options.nobrace

Disable expansion of brace patterns. Same behavior as [minimatch](https://github.com/isaacs/minimatch) `nobrace`.

Type: `Boolean`

Default: `undefined`

See [braces](https://github.com/jonschlinkert/braces) for more information about extended brace expansion.

### options.nodupes

Remove duplicate elements from the result array.

Type: `Boolean`

Default: `undefined`

**Example**

Example of using the `unescape` and `nodupes` options together:

```js
mm.match(['a/b/c', 'a/b/c'], 'a/b/c');
//=> ['a/b/c', 'a/b/c']

mm.match(['a/b/c', 'a/b/c'], 'a/b/c', {nodupes: true});
//=> ['abc']
```

### options.nocase

Use a case-insensitive regex for matching files. Same behavior as [minimatch](https://github.com/isaacs/minimatch).

Type: `Boolean`

Default: `undefined`

### options.nonegate

Disallow negation (`!`) patterns, and treat leading `!` as a literal character to match.

Type: `Boolean`

Default: `undefined`

### options.nonull

If `true`, when no matches are found the actual (arrayified) glob pattern is returned instead of an empty array. Same behavior as [minimatch](https://github.com/isaacs/minimatch).

Type: `Boolean`

Default: `undefined`

### options.unixify

Normalize slashes in file paths and glob patterns to forward slashes.

Type: `Boolean`

Default: `undefined` on non-windows, `true` on windows.

### options.cache

Disable memoization of matching functions and generated regex.

Type: `Boolean`

Default: `undefined`

## Notes

**Bash 4.3 parity**

Whenever possible parsing behavior for patterns is based on globbing specifications in Bash 4.3. Patterns that aren't described by Bash follow wildmatch spec (used by git).

**Escaping**

Backslashes are exclusively and explicitly reserved for escaping characters in a glob pattern, even on windows. This is also the case in minimatch and node-glob, although some users are [confused about how this works](https://github.com/isaacs/node-glob/issues/212).

To be clear, _a glob pattern is not a filepath_. When you pass something like `path.join('foo', '*')` to micromatch, you are creating a filepath and expecting for it to work as a glob pattern. The problem is that on windows the node.js `path` module converts `/` to `\\` and/or uses `\\` as the path separator when joining paths. But since `\\` is an escape character in globs, this means that on windows `path.join('foo', '*')` results in `foo\\*` which tells micromatch that `*` is escaped.

Thus you'll need to either do the joining manually or find a lib on npm that does this.

## Benchmarks

Run the [benchmarks](./benchmark):

```bash
node benchmark
```

As of November 03, 2016 (longer bars are better):

```sh
# braces-globstar-large-list
micromatch ██████████████████████████████████████████████████████████████████████ (395 ops/sec) 
minimatch  ██ (13.75 ops/sec) 
multimatch ██ (13.90 ops/sec) 

# braces-multiple
micromatch ██████████████████████████████████████████████████████████████████████ (35,630 ops/sec) 
minimatch   (1.58 ops/sec) 
multimatch  (1.47 ops/sec) 

# braces-range
micromatch ██████████████████████████████████████████████████████████████████████ (192,168 ops/sec) 
minimatch  ██ (8,167 ops/sec) 
multimatch ██ (8,227 ops/sec) 

# braces-set
micromatch ██████████████████████████████████████████████████████████████████████ (24,648 ops/sec) 
minimatch  █████ (1,960 ops/sec) 
multimatch █████ (1,841 ops/sec) 

# globstar-large-list
micromatch ██████████████████████████████████████████████████████████████████████ (396 ops/sec) 
minimatch  ████ (25.49 ops/sec) 
multimatch ████ (25.61 ops/sec) 

# globstar-long-list
micromatch ██████████████████████████████████████████████████████████████████████ (3,337 ops/sec) 
minimatch  ██████████ (504 ops/sec) 
multimatch ██████████ (493 ops/sec) 

# globstar-short-list
micromatch ██████████████████████████████████████████████████████████████████████ (465,408 ops/sec) 
minimatch  ████ (32,763 ops/sec) 
multimatch ████ (28,196 ops/sec) 

# no-glob
micromatch ██████████████████████████████████████████████████████████████████████ (642,909 ops/sec) 
minimatch  ███ (33,682 ops/sec) 
multimatch ███ (29,675 ops/sec) 

# star-basename
micromatch ██████████████████████████████████████████████████████████████████████ (11,012 ops/sec) 
minimatch  ███████████████████ (3,076 ops/sec) 
multimatch ███████████████████ (3,001 ops/sec) 

# star
micromatch ██████████████████████████████████████████████████████████████████████ (9,786 ops/sec) 
minimatch  █████████████████████ (2,976 ops/sec) 
multimatch ███████████████████ (2,791 ops/sec) 
```

## About

### Related projects

* [braces](https://www.npmjs.com/package/braces): Fast, comprehensive, bash-like brace expansion implemented in JavaScript. Complete support for the Bash 4.3 braces… [more](https://github.com/jonschlinkert/braces) | [homepage](https://github.com/jonschlinkert/braces "Fast, comprehensive, bash-like brace expansion implemented in JavaScript. Complete support for the Bash 4.3 braces specification, without sacrificing speed.")
* [expand-brackets](https://www.npmjs.com/package/expand-brackets): Expand POSIX bracket expressions (character classes) in glob patterns. | [homepage](https://github.com/jonschlinkert/expand-brackets "Expand POSIX bracket expressions (character classes) in glob patterns.")
* [extglob](https://www.npmjs.com/package/extglob): Extended glob support for JavaScript. Adds (almost) the expressive power of regular expressions to glob… [more](https://github.com/jonschlinkert/extglob) | [homepage](https://github.com/jonschlinkert/extglob "Extended glob support for JavaScript. Adds (almost) the expressive power of regular expressions to glob patterns.")
* [fill-range](https://www.npmjs.com/package/fill-range): Fill in a range of numbers or letters, optionally passing an increment or `step` to… [more](https://github.com/jonschlinkert/fill-range) | [homepage](https://github.com/jonschlinkert/fill-range "Fill in a range of numbers or letters, optionally passing an increment or `step` to use, or create a regex-compatible range with `options.toRegex`")
* [nanomatch](https://www.npmjs.com/package/nanomatch): Fast, minimal glob matcher for node.js. Similar to micromatch, minimatch and multimatch, but complete Bash… [more](https://github.com/jonschlinkert/nanomatch) | [homepage](https://github.com/jonschlinkert/nanomatch "Fast, minimal glob matcher for node.js. Similar to micromatch, minimatch and multimatch, but complete Bash 4.3 wildcard support only (no support for exglobs, posix brackets or braces)")

### Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

Please read the [contributing guide](.github/contributing.md) for avice on opening issues, pull requests, and coding standards.

### Contributors

| **Commits** | **Contributor**<br/> | 
| --- | --- |
| 345 | [jonschlinkert](https://github.com/jonschlinkert) |
| 12 | [es128](https://github.com/es128) |
| 3 | [paulmillr](https://github.com/paulmillr) |
| 2 | [TrySound](https://github.com/TrySound) |
| 2 | [doowb](https://github.com/doowb) |
| 2 | [tunnckoCore](https://github.com/tunnckoCore) |
| 2 | [MartinKolarik](https://github.com/MartinKolarik) |
| 1 | [amilajack](https://github.com/amilajack) |
| 1 | [UltCombo](https://github.com/UltCombo) |
| 1 | [tomByrer](https://github.com/tomByrer) |

### Building docs

_(This document was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme) (a [verb](https://github.com/verbose/verb) generator), please don't edit the readme directly. Any changes to the readme must be made in [.verb.md](.verb.md).)_

To generate the readme and API documentation with [verb](https://github.com/verbose/verb):

```sh
$ npm install -g verb verb-generate-readme && verb
```

### Running tests

Install dev dependencies:

```sh
$ npm install -d && npm test
```

### Author

**Jon Schlinkert**

* [github/jonschlinkert](https://github.com/jonschlinkert)
* [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

### License

Copyright © 2016, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT license](https://github.com/jonschlinkert/micromatch/blob/master/LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.2.0, on November 03, 2016._