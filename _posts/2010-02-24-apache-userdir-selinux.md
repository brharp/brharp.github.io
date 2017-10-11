---
title: Apache UserDir and SELinux
layout: post
---
After enabling UserDir and setting the necessary file permissions, run:
```
restorecon -R /home
```
