---
title: Performance tesing metrics design
layout: post
date: '2020-04-20'
---
# Goals
* Benchmark
* Scalability
* Stability
* Hunting bottlenecks

# Questions
* Do we have specs? WRITE DOWN acceptance criteria BEFORE running the test
* Are there bottlenecks outside?
* Has it gone through functional?

# Proposed flow `__/^^^\__`
* pre-flight
* heating up
* cruising
* cooling down
* post-load observations

# 2 kinds of metrics
* black box
* telemetry

# Telemtry reporting
* there is always a probe
* probe footprint

# Fighting noise and distortions (be a useful engine)
* reproducibility, comparability
* extra traffic, cpu, mem, IO, cache, IPC
* spikes
* measure host+system overall vs exact process

# Metrics representation
* relative vs cumulative(!)  
  Cumulative not always works (eg median calcualtion)  
  Some relative metrics can be unexpectedly easy derived
  `curr_conn_count=total_conn-total_disconn`
* latency vs throughput

# Metrics normalization
* bind everything to timestamps (to mitigate delays, batch reporting etc)
* make sure clocks in sync (out of sync measurements caused a lot of issues in history, eg amper induction story)
* note network delays if no clock
* agreeing diverse data sources

# Alerts
* coredumps
* process restarts
* system health (eg SSD IO degrades due to lack of free space)

# Post-processing and analysis
* average is deceptive (use various statistics analysis techniques, hire someone)
  but useful as noise removal-trend-exposing tool
* derivatives (partial, 1st, 2nd)
* peak vs worst vs percentiles
* correlations and inter-relations (something per something)
* cause-effects (hint: look who is first: cause sometimes observed earlier)
* extrapolations and assumptioins (it's all about models, beware of broken ones)