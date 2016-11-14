# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

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
