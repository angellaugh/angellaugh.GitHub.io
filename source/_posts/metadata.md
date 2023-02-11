---
layout: posts
title: metadata
date: 2023-02-11 20:42:46
tags: metadata 
categories: Teckknowledge
---


#### ask Pytest to automatically provision the environment
`pytest --provision`
```
ali1@remotedev-ali1-rdu:~$ pytest --provision
logdir 'log/helix' does not exist; fallback to '/ifs/home/ali1/.cache/helix/log'
============================================================================== test session starts ===============================================================================
platform linux -- Python 3.6.9, pytest-6.2.4, py-1.10.0, pluggy-0.13.1
rootdir: /ifs/home/ali1
plugins: timeout-1.4.2, helix-3.0.25, isilon-marks-0.3.3, isilon-accountant-0.4.2
collected 0 items / 1 error
Password:
[2022-12-14 11:11:32.605863+00:00] QUEUED_FOR_ALLOCATION... waiting 30 seconds
[2022-12-14 11:12:02.887701+00:00] QUEUED_FOR_ALLOCATION... waiting 30 seconds
[2022-12-14 11:12:33.175448+00:00] READY
[2022-12-14 11:12:33.499574+00:00] QUEUED_FOR_DELETION... waiting 30 seconds
```

