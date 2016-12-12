[![npm version](https://img.shields.io/npm/v/nws-cli.svg)](https://npmjs.com/package/nws-cli) [![license](https://img.shields.io/npm/l/nws-cli.svg)](https://github.com/mklement0/nws-cli/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [nws &mdash; whitespace normalization](#nws-&mdash-whitespace-normalization)
- [Examples](#examples)
  - [Transliteration Examples](#transliteration-examples)
  - [Condensing Examples](#condensing-examples)
- [Installation](#installation)
  - [Installation from the npm registry](#installation-from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# nws &mdash; whitespace normalization

`nws` is a Unix CLI that normalizes whitespace in text, offering several modes,
grouped into two categories:

* Whitespace _transliteration_ modes:

Line endings can be changed to be Windows- or Unix-specific, and select  
Unicode whitespace and punctuation can be replaced with their closest ASCII  
equivalents.

* Whitespace _condensing_ modes:

Trims leading and trailing runs of any mix of tabs and spaces and replaces  
them with a single space each. The individual modes in this category differ 
only with respect to how _multi-line_ input is treated. 

Input can be provided either via filename arguments or via stdin.
Option `-i` offers in-place updating.

See the examples below, get concise [usage](#usage) information further below,
or read the [manual](doc/nws.md).

# Examples

Note:

## Transliteration Examples

```shell
# Converts a CRLF line-endings file (Windows) to a LF-only file (Unix).
# No output is produced, because the file is updated in-place; a backup
# of the original file is created with suffix '.bak'. 
$ nws --mode lf --in-place=.bak from-windows.txt

# Converts a LF-only file (Unix) to a CRLF line-endings file (Windows).
# No output is produced, because the file is updated in-place; since no
# backup suffix is specified, no backup file is created.
$ nws --crlf -i from-unix.txt

# Converts select Unicode whitespace and punctuation chars. to their 
# closest ASCII equivalents and sends the output to a different file.
# Note that any other non-ASCII characters are left untouched.
# Helpful for converting code samples that were formatted for display back to
# valid source code. 
$ nws --ascii unicode-punct.txt > ascii-punct.txt 

```

## Condensing Examples

 * Output from the example commands is piped to `cat -et` to better illustrate the output; `cat -et` shows line endings as `$` (and control chars. as `^M<char>`; e.g., a tab would show as `^I`).

```shell
# -- Single-input-line normalization (mode option doesn't apply).

> nws <<<'    I   will   be normalized.   ' | cat -et 
I will be normalized.$
  # Ditto, but with a mix of spaces and tabs.
> nws "$(printf ' I \t\t will   be normalized.\t\t')" | cat -et 
I will be normalized.$

# -- Multi-input-line normalizations, using different modes.

  # Create demo file.
> cat <<EOF > /tmp/nws-demo

    $(printf '\t')
  
one
  two  

   $(printf '\t')

three


EOF

  # Multi-paragraph mode - by default, or with `--mp` or `-m mp` or 
  # `--mode multi-para`.
  # In addition to line-internal normalization, 
  # folds runs of blank/empty lines into 1 empty line each.
$ nws < /tmp/nws-demo | cat -et
$
one$
two$
$
three$
$

  # Single-paragraph mode: `--sp` or `-m sp` or `--mode single-para`
  # In addition to line-internal normalization, 
  # removes all blank/empty lines.
$ nws --sp < /tmp/nws-demo | cat -et
one$
two$
three$

  # Flattened-multi-pargraph mode: `--fp` or `-m fp` or `--mode flat-para`
  # In addition to line-internal normalization, 
  # joins paragraph-internal lines with a space each.
$ nws --fp < /tmp/nws-demo | cat -et
$
one two$
$
three$
$

  # Single-output-line mode: `sl` or `-m sl` or `--mode single-line`.
  # In addition to line-internal normalization, 
  # joins all non-empty/non-blank lines with a space each
  # to form a single, long output line.
$ nws --sl < /tmp/nws-demo | cat -et
one two three$
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** and **POSIX-compatible utilities**.

## Installation from the npm registry

<sup>Note: Even if you don't use Node.js, its package manager, `npm`, works across platforms and is easy to install; try [`curl -L http://git.io/n-install | bash`](https://github.com/mklement0/n-install)</sup>

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

Find concise usage information below; for complete documentation, read the [manual online](doc/nws.md) or,
once installed, run `man nws` (`nws --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ nws --help


Normalizes whitespace in one of several modes.

    nws [-m <mode>] [[-i[<ext>]] file...]

    Condensing <mode>s:

    All these modes normalize runs of tabs and spaces to a single space  
    each and trim leading and trailing runs; they only differ with respect to
    how multi-line input is processed.

    mp   (default) multi-paragraph: folds multiple blank lines into one
    fp   flattened multi-paragraph: normalizes each paragraph to single line
    sp   single-paragraph: removes all blank lines.
    sl   single-line: normalizes to single output line

    Transliteration <mode>s:

    lf     translates line endings to LF-only (\n)
    crlf   translates line endings to CRLF (\r\n)
    ascii  translates Unicode whitespace and punctuation to ASCII

Alternatively, specify mode values directly as options; e.g., --sp in lieu  
of -m sp

Standard options: --help, --man, --version, --home
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2016 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.3.2](https://github.com/mklement0/nws-cli/compare/v0.3.1...v0.3.2)** (2016-12-11):
  * [fix] Mode `--crlf` is now idempotent with input that is already CRLF-
          terminated (previously, an extra CR was mistakenly added).

* **[v0.3.1](https://github.com/mklement0/nws-cli/compare/v0.3.0...v0.3.1)** (2016-12-10):
  * [doc] Copy-editing in read-me file.

* **[v0.3.0](https://github.com/mklement0/nws-cli/compare/v0.2.0...v0.3.0)** (2016-11-13):
  * [BREAKING CHANGE] `nws` is now file-based: operands are interpreted as 
    filenames, and option `-i` allows in-place updating. Use stdin to provide
    strings as input, such as via `echo ... | nws ...`.
  * [enhancement] New transliteration modes added for changing line-ending
    styles and for translating non-ASCII Unicode whitespace/punctuation to
    their closest ASCII equivalents.

* **[v0.2.0](https://github.com/mklement0/nws-cli/compare/v0.1.4...v0.2.0)** (2015-09-18):
  * [usability improvement] New, mnemonic mode names supersede the old numeric 
    normalization modes (option-arguments for `-m`); mode names come in both
    short and long forms; similarly, `--mode` is now supported as a verbose
    alternative to `-m`.
  * [deprecation] The numeric modes (0..3) still work, but should no longer be
    used and are no longer documented.
  * [doc] `nws` now has a man page (if manually installed, use `nws --man`);
    `nws -h` now just prints concise usage information.

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
