---
title: Test automation smells
layout: post
tags: [qa, pytest]
---

1. Missing code convention, styles, docsstrings, DRY, SOLID, etc.
	* Detect: run linters
	* Fix: Treat autotests as a real code too. Write separate code convention (eg: pylint configs). Periodic code reviews for tests (can be automated)
1. Using `time.sleep()` (especially in async code)
	* Detect: search code
	* Fix: wait for events, markers, flags, conditions (polling)
1. Using `print()` or `print-print-print-assert`
	* Detect: search code
	* Fix: consider logging (delegate log setup to test runner!)
1. Using `..except: pdb.set_trace()`
	* Detect: search code
	* Fix: consider `--pdb` flag
1. `True != False` (obvious and useless obscure results)
	* Detect: make tests fail, see the output
	* Fix: Use error message (assert 2nd param)
1. `True == False` (happy blind tests)
	* Detect: make tests fail
	* Fix: review test-plan, use `pytest.mark.xfail` (or skip)
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
