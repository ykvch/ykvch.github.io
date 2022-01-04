---
title: Test Framework
layout: post
date: '2022-01-03'
---

## Test lab
* controllable / observable
* reproducible / predictable
* disposable / rapidly deployable

## Parts of framework:
### Essential:
* test framework config (CLI, files, env)
* SUT management (separate! Derived from above)
* logging
* runner
* reporting

### To make it actually work:
* same but meaningful...
* environment/platform interaction
* runtime setup
* mocks
* monitoring
* validators
* test data/artifacts management
* secrets management
* data generators
* debugging
* versioning
* UI
* documentation (help text)

## Style:
* fixtures should represent states and resources (NOT actions! Idemopotency FTW)
* fixtures should target SUT and its environment
* plugins should target test process and framework itself
* do NOT intermix unit/component/integration/e2e tests
* steps style as `actor.action(target, options)`