[![npm version](https://img.shields.io/npm/v/nws-cli.svg)](https://npmjs.com/package/nws-cli) [![license](https://img.shields.io/npm/l/nws-cli.svg)](https://github.com/mklement0/nws-cli/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [nws &mdash; introduction](#nws-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [From the npm registry](#from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# nws &mdash; introduction

`nws` is a Unix CLI that normalizes whitespace in text, offering several modes.

Input can be provided either via arguments or via stdin.

All modes behave identically with respect to normalizing a given _single_ line:  
Any run of any mix of spaces and tabs is replaced with a single space each, except leading and trailing runs, which are discarded.

The mode matters with multi-line input; see the [examples](#examples) below or the [usage](#usage) chapter for details.

# Examples

Note:

 * Output from the example commands is piped to `cat -et` to better illustrate the output; `cat -et` shows line endings as `$` (and control chars. as `^M<char>`; e.g., a tab would show as `^I`).

```shell
  # Single-line normalization
> nws '    I   will   be normalized.   ' | cat -et 
I will be normalized.$
  # Ditto, but with a mix of spaces and tabs.
> nws "$(printf ' I \t\t will   be normalized.\t\t')" | cat -et 
I will be normalized.$

  # Create demo file.
> cat <<EOF > /tmp/nws-demo

    $(printf '\t')
  
one
  two  

   $(printf '\t')

three


EOF

  # Multi-line normalization in default mode, 0:
  # In addition to line-internal normalization, 
  # folds runs of blank/empty lines into 1 empty line each.
> nws < /tmp/nws-demo | cat -et
$
one$
two$
$
three$
$

  # Multi-line normalization in mode 1:
  # In addition to line-internal normalization, 
  # removes all blank/empty lines.
> nws -m 1 < /tmp/nws-demo | cat -et
one$
two$
three$

  # Multi-line normalization in mode 2:
  # In addition to line-internal normalization, 
  # joins paragraph-internal lines with a space each.
> nws -m 2 < /tmp/nws-demo | cat -et
$
one two$
$
three$
$

  # Multi-line normalization in mode 3:
  # In addition to line-internal normalization, 
  # joins all non-empty/non-blank lines with a space each
  # to form a single, long line.
> nws -m 3 < /tmp/nws-demo | cat -et
one two three$
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** and **POSIX-compatible utilities**.

## From the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/nws-cli) as follows:

    [sudo] npm install nws-cli -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `nws` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/nws-cli/stable/bin/nws) as `nws`.
* Make it executable with `chmod +x nws`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).

# Usage

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ nws --help

SYNOPSIS
  nws [-m mode] [text]

DESCRIPTION
  nws (*n*ormalize *w*hite*s*pace) performs whitespace normalization,
  offering several modes.
  
  Input is provided via one or more operands or, in their absence, via stdin.
  To disambiguate operands from options, precede operands with '--' as a
  separate argument.
  
  Note that specifying a mode with -m applies only to *multi-line* input.
  All modes behave identically with respect to normalizing a given single
  line:
    Any run of any mix of spaces and tabs is replaced with a single space
    each, except leading and trailing runs, which are discarded.

  -m mode
    0 (default) ... runs of blank (all-whitespace or empty) lines are
      replaced with 1 empty line each, resulting in paragraph-internal
      newlines getting preserved, with blank lines at the beginning, between
      paragraphs, and at the end getting normalized to a single empty line
      each.
    1 ... runs of blank (all-whitespace or empty) lines are discarded,
      resulting in a single block of non-blank lines.
    2 ... like mode 0, except that paragraph-internal newlines are replaced
      with a single space each, resulting in each paragraph becoming a
      single line, with 1 empty line between paragraphs.
    3 ... normalization includes newlines too, so that any run of any mix of
      spaces, tabs, and newlines is replaced with a single space each,
      resulting in a single, long output line.

EXAMPLES
  nws $'  one \t\t two  three   ' # -> 'one two three'
  nws <<<$'\n\n  one\n two \n\n\n  three\n\n' # -> $'\none\ntwo\n\nthree\n'
  nws -m 1 <<<$'\n\n  one\n two \n\n\n  three\n\n' # -> $'one\ntwo\nthree'
  nws -m 2 <<<$'\n\n  one\n two \n\n\n  three\n\n' # -> $'\none two\nthree\n'
  nws -m 3 <<<$'  one two\n  three ' # -> 'one two three'
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.1.4](https://github.com/mklement0/nws-cli/compare/v0.1.3...v0.1.4)** (2015-09-15):
  * [dev] Makefile improvements; various other behind-the-scenes tweaks.

* **[v0.1.3](https://github.com/mklement0/nws-cli/compare/v0.1.2...v0.1.3)** (2015-06-13):
  * [doc] Read-me improvements.

* **[v0.1.2](https://github.com/mklement0/nws-cli/compare/v0.1.1...v0.1.2)** (2015-06-13):
  * [doc] Read-me improvements.

* **[v0.1.1](https://github.com/mklement0/nws-cli/compare/v0.1.0...v0.1.1)** (2015-06-13):
  * [doc] Read-me improvements.

* **v0.1.0** (2015-06-13):
  * Initial release.
