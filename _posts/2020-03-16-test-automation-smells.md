---
title: Test automation smells
layout: post
tags: [qa, pytest]
---

1. No code convention, styles, docs-comments, coding practices (DRY SOLID etc).
    * Detect: run linters
    * Fix: Treat autotests as a real code too. Write separate code convention (eg: pylint configs). Periodic code reviews for tests (can be automated)
1. Using `time.sleep()` (especially in async code)
1. `True != False` (obvious and useless obscure results)
1. `True == False` (simply blind tests)
1. Too many asserts (use well-defined validators)
1. Failing outside _expected behaviour_ checks (everybody fail now, bam bam bam bam)
1. Focusing outside SUT (sometimes requires peeking into SUT code structure)