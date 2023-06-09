
# shUnit2 2.1.x Changes

## Changes with 2.1.9

### Fixed

Issue #37. shUnit2 now works properly for tests that have the `-e` shell option enabled. This took *way* longer than originally anticipated, especially once older Ubuntu releases were added to the testing on Travis CI.

Issue #129. `assertFalse ''` now returns `SHUNIT_TRUE`.

Issue #54 (again). To fix #37, this fix had to be reworked to use 'builtin' instead of 'command' as the latter caused problems with `set -e`.

## Changes with 2.1.8

### New

Issue #29. Add support for user defined prefix for test names. A prefix can be added by defining the `SHUNIT_TEST_PREFIX` variable.

Issue #59. Added `assertContains` and `assertNotContains` functionality.

### Improvements

Issue #78. Added an example for using suite tests.

Run continuous integration additionally against Ubuntu Trusty.

### Fixed

Issue #94. Removed the `gen_test_report.sh` script as the Travis CI output can be used instead. Reports were used before Travis CI was used.

Issue #84. Treat syntax errors in functions as test failures.

Issue #77. Fail tests when the environment functions (e.g. `setup()` or`tearDown()`) fail.


## Changes with 2.1.7

### Bug fixes

Issue #69. shUnit2 should not exit with 0 when it has (syntax) errors.

### Enhancements

Issue #54. Shell commands prefixed with '\' so that they can be stubbed in tests.

Issue #68. Ran all code through [ShellCheck](http://www.shellcheck.net/).

Issue #60. Continuous integration tests now run with [Travis CI](https://travis-ci.org/kward/shunit2).

Issue #56. Added color support. Color is enabled automatically when supported, but can be disabled by defining the `SHUNIT_COLOR` environment variable before sourcing shunit2. Accepted values are `always`, `auto` (the default), and `none`.

Issue #35. Add colored output.

### Other

Moved code to [GitHub](https://github.com/kward/shunit2), and restructured to be more GitHub like.

Changed to the Apache 2.0 license.


## Changes with 2.1.6

Removed all references to the DocBook documentation.

Simplified the 'src' structure.

Fixed error message in `fail()` that stated wrong number of required arguments.

Updated `lib/versions`.

Fixed bug in `_shunit_mktempDir()` where a failure occurred when the 'od' command was not present in `/usr/bin`.

Renamed `shunit_tmpDir` variable to `SHUNIT_TMPDIR` to closer match the standard `TMPDIR` variable.

Added support for calling shunit2 as an executable, in addition to the existing method of sourcing it in as a library. This allows users to keep tests working despite the location of the shunit2 executable being different for each OS distribution.

Issue #14: Improved handling of some strange chars (e.g. single and double quotes) in messages.

Issue# 27: Fixed error message for `assertSame()`.

Issue# 25: Added check and error message to user when phantom functions are written to a partition mounted with `noexec`.

Issue# 11: Added support for defining functions like `function someFunction()`.


## Changes with 2.1.5

Issue# 1: Fixed bug pointed out by R Bernstein in the trap code where certain types of exit conditions did not generate the ending report.

Issue# 2: Added `assertNotEquals()` assert.

Issue# 3: Moved check for unset variables out of shUnit2 into the unit tests. Testing poorly written software blows up if this check is in, but it is only interesting for shUnit2 itself. Added `shunit_test_output.sh` unit test for this. Some shells still do not catch such errors properly (e.g. Bourne shell and BASH 2.x).

Added new custom assert in test_helpers to check for output to STDOUT, and none to STDERR.

Replaced fatal message in the temp directory creation with a `_shunit_fatal()` function call.

Fixed `test_output` unit test so it works now that the `set -u` stuff was removed for Issue# 3.

Flushed out the coding standards in the `README.txt` a bit more, and brought the shunit2 code up to par with the documented standards.

Issue# 4: Completely changed the reporting output to be a closer match for JUnit and PyUnit. As a result, tests are counted separately from assertions.

Provide public `shunit_tmpDir` variable that can be used by unit test scripts that need automated and guaranteed cleanup.

Issue# 7: Fixed duplicated printing of messages passed to asserts.

Per code review, fixed wording of `failSame()` and `failNotSame()` messages.

Replaced `version_info.sh` with versions library and made appropriate changes in other scripts to use it.

Added `gen_test_results.sh` to make releases easier.

Fixed bugs in `shlib_relToAbsPath()` in shlib.

Converted DocBook documentation to reStructuredText for easier maintenance. The DocBook documentation is now considered obsolete, and will be removed in a future release.

Issue# 5: Fixed the documentation around the usage of failures.

Issue# 9: Added unit tests and updated documentation to demonstrate the requirement of quoting values twice when macros are used. This is due to how shell parses arguments.

When an invalid number of arguments is passed to a function, the invalid number is returned to the user so they are more aware of what the cause might be.


## Changes with 2.1.4

Removed the `_shunit_functionExists()` function as it was dead code.

Fixed zsh version number check in `version_info`.

Fixed bug in last resort temporary directory creation.

Fixed off-by-one in exit value for scripts caught by the trap handler.

Added argument count error checking to all functions.

Added `mkdir_test.sh` example.

Moved `src/test` into `src/shell` to better match structure used with shFlags.

Fixed problem where null values were not handled properly under ksh.

Added support for outputting line numbers as part of assert messages.

Started documenting the coding standards, and changed some variable names as a result.

Improved zsh version and option checks.

Renamed the `__SHUNIT_VERSION` variable to `SHUNIT_VERSION`.


## Changes with 2.1.3

Added some explicit variable defaults, even though the variables are set, as they sometimes behave strange when the script is canceled.

Additional workarounds for zsh compatibility.

shUnit2 now exits with a non-zero exit code if any of the tests failed. This was done for automated testing frameworks. Tests that were skipped are not considered failures, and do not affect the exit code.

Changed detection of STDERR output in unit tests.


## Changes with 2.1.2

Unset additional variables that were missed.

Added checks and workarounds to improve zsh compatibility.

Added some argument count checks `assertEquals()`, `assertNull()`, and
`assertSame()`.


## Changes with 2.1.1

Fixed bug where `fail()` was not honoring skipping.

Fixed problem with `docs-docbook-prep` target that prevented it from working. (Thanks to Bryan Larsen for pointing this out.)

Changed the test in `assertFalse()` so that any non-zero value registers as false. (Credits to Bryan Larsen)

Major fiddling to bring more in line with [JUnit](http://junit.org/). Asserts give better output when no message is given, and failures now just fail.

It was pointed out that the simple 'failed' message for a failed assert was not only insufficient, it was nonstandard (when compared to JUnit) and didn't provide the user with an expected vs actual result. The code was revised somewhat to bring closer into alignment with JUnit (v4.3.1 specifically) so that it feels more "normal". (Credits to Richard Jensen)

As part of the JUnit realignment, it was noticed that `fail*()` functions in JUnit don't actually do any comparisons themselves. They only generate a failure message. Updated the code to match.

Added self-testing unit tests. Kinda horkey, but they did find bugs during the JUnit realignment.

Fixed the code for returning from asserts as the return was being called before the unsetting of variables occurred. (Credits to Mathias Goldau)

The assert(True|False)() functions now accept an integer value for a conditional test. A value of '0' is considered 'true', while any non-zero value is considered 'false'.

All public functions now fill use default values to work properly with the '-x' shell debugging flag.

Fixed the method of percent calculation for the report to get achieve better accuracy.


## Changes with 2.1.0 (since 2.0.1)

This release is a branch of the 2.0.1 release.

Moving to [reStructured Text](http://docutils.sourceforge.net/rst.html) for the documentation.

Fixed problem with `fail()`. The failure message was not properly printed.

Fixed the `Makefile` so that the DocBook XML and XSLT files would be downloaded before parsing can continue.

Renamed the internal `__SHUNIT_TRUE` and `__SHUNIT_FALSE` variables to`SHUNIT_TRUE` and `SHUNIT_FALSE` so that unit tests can "use" them.

Added support for test "skipping". If skipping is turned on with the `startSkip()` function, `assert` and `fail` functions will return immediately, and the skip will be recorded.

The report output format was changed to include the percentage for each test result, rather than just those successful.
