---
title: Test automation smells
layout: post
tags: [qa, pytest]
---

1. Lack of code convention, uniform style, docstrings, coding principles (DRY, SOLID, etc).
  * Detect: run linters
  * Fix: Treat autotests as a code too. Write separate code convention (eg: pylint configs). Periodic code reviews for tests should be automated.
1. Using `time.sleep()` (especially in async code)
  * Detect: search code
  * Fix: wait for events, markers, flags, conditions (polling)
1. Using `print()` or `print-print-print-assert`
  * Detect: search code
  * Fix: consider logging (delegate log setup to test runner!)
1. Using `..except: pdb.set_trace()`
  * Detect: search code
  * Fix: consider `--pdb` flag
1. `True == False` (happy blind tests)
  * Detect: make tests fail
  * Fix: review test-plan, use `pytest.mark.xfail` (or skip)
1. `True != False` (obvious and useless obscure results) missing value explanation
  * Detect: make tests fail, see the output
  * Fix: Use error message (assert 2nd param)
1. Hardcoding without explanation (in test cases _DO describe, DON'T define_).
  > Note: hardcoding here is used in broader meaining, as kind of source for "semantic satiation" in code.
  * Detect: There are exact values, steps, behaviour without ability to trace WHY are those chosen and what logic (specs?) are considered behind them.
  * Fix: use data generators with rules derived from specs (also helps mitigate pesticide effect a little).
  * Fix: don't be too implementatin specific (eg: use 'turn lights on' instead of 'flip switch up').
  * Fix: comments and docstrings might help.
1. Missing (hard to retrive) breadcrumbs (thread id, timestamp, case name, uuid, log/code line numbers... etc) during debugging/investigation/results analysis.
  * Detect: See if _advertised_ error message can easily lead to point in test/environment WHERE unexpected behaviour happened.
  * Fix: Add bits (href-s, id-s, timesamps...) to easily track back to error point.
1. Too many asserts
  * Detect: count asserts in tests
  * Fix: pre-defined validators, prepare results for validation, soft-asserts
1. Failing outside _expected behaviour_ checks
  * Detect: review how many tests fail on test run errors
  * Fix: assert only target feature (consider fixtures in pytest OR raising
	non AssertionError in unittest-likes)
1. Focusing outside SUT
  * Detect: check if SUT interaction present in tests (sometimes also requires peeking into SUT code structure)
  * Fix: tests review, test-plans review, dev assistance
1. Branching/nested (non-parameterized) logic in steps (and expected results).
  * Detect: search code (and docstrings) for if-statements
  * Fix: proper parametrize, split cases
  * Fix: prohibit If-else statements (nesting non parameterizable logic) inside test case.
1. Workarounds for unstable run conditions
  * Detect: fuzzy/conditional handling of permanent steps
  * Fix: Abstract, narrow down
1. Parameters deep nesting, generic variable names
  * Detect: option, value... smell-words
  * Fix: Avoid generic variable/parameters naming (eg: option, value).
  * Fix: Keep parameterise as flat as possible.
1. Unclear docstring logic
  * Detect: Try to reproduce docstring steps manually.
  * Fix: Use `<actor> <action> <options>`.
1. Missing logs BEFORE risky operations and/or at the brim of setup/steps/teardown.
  * Detect: run tests and see if You can clearly distinguish test phase boundaries
1. Missing asserts and/or validations in test case entirely.
1. Fixture flood (there are more fixtures than test functions)
  * Detect: compare

    ```cd tests/; grep -r --include='*.py' '@.*fixture' . | wc -l```
    
	vs
    
	```grep -r --include='*.py' 'def test_' . | wc -l```
  	
	If former is bigger than the latter, chances are fixtures got misused.
  
  * Fix: Refactor fixtures into meaningful pythonic reusable modules. Parametrize fixtures. RTFM, think hard of what fixtures are meant for.
1. God fixture.

# Short summary

```
Docstring should be reproducible manually (to some extent).
Avoid generic variable/parameters naming (eg: option, value).
Keep parameterise as flat as possible.
If-else statements (nesting non parameterizable logic) are prohibited inside test case.
```
