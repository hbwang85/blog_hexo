---
title: hexo bind DNS encounter 404 error
date: 2016-12-21 18:50:21
tags: dns
---

Every time I post new articles to blog, my dns will be **404 error**

My DNS config is as below:
![](img/dnsconfig.png)

After google I found the root cause is every time I deploy, the `CNAME` file will be deleted.

This file should be under your root path. you can put it in the `source` folder, then it will be exist for ever.




