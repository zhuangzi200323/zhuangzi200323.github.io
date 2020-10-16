---
title: startActivityForResult使用时注意事项
date: 2010-06-02 15:06:15
img: '/medias/3.jpg'
categories:
- Android技术相关
tags: 
- Android
---

## startActivityForResult使用时注意事项

startActivityForResult时，不要加FLAG_ACTIVITY_NEW_TASK flag，否则会立即进入onActivityResult