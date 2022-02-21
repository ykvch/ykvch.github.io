---
title: Test Framework
layout: post
date: '2022-01-03'
---

## Test environment (lab, stand)
* controllable / observable
* reproducible / predictable ([producing reliable results is far more important that just results themseleves](https://twitter.com/CormacORafferty/status/1493463979041173505)))
* disposable / rapidly deployable

## Parts of framework:
### Essential:
* test framework configuration (as fallback dicts: CLI->config files->env variables)
* SUT management (separate module! Derived from above)
* logging
* runner
* reporting/aggregation

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
* fixtures should represent states and resources (NOT actions! Idempotency FTW)
* fixtures should target SUT and its environment
* plugins should target test process and framework itself
* do NOT intermix unit/component/integration/e2e tests
* steps style as `actor.action(target, options)`