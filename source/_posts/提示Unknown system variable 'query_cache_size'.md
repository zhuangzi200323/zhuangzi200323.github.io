---
title: 提示Unknown system variable 'query_cache_size'
date: 2019-06-02 15:06:15
img: '/medias/11.jpg'
categories:
- spring
---

## title: 提示Unknown system variable 'query_cache_size'

提示这个错误：Unknown system variable 'query_cache_size'

原因：使用mysql-connector-java的版本与mysql版本不兼容

解决办法是升级mysql-connector-java版本或降级mysql版本