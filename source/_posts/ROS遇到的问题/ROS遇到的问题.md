---
title: ROS遇到的问题
date: 2020-03-02 15:06:15
img: '/medias/7.jpg'
categories:
- ROS
---

## 1. sudo rosdep init与rosdep update出错

因为raw.Githubusercontent.com无法访问，去下方链接可以查询raw.Githubusercontent.com的ip，将ip加入到hosts中再试即可

ip查询网站 <https://site.ip138.com/raw.Githubusercontent.com/>

输入 raw.githubusercontent.com 查询IP地址

![clipboard.png](media/80ff70bf829de99dfecaa3785dba3820.png)

添加 该ip 到hosts文件 sudo vi /etc/hosts

![clipboard.png](media/214deb83259203c1702aec0ac1120dff.png)

https://blog.csdn.net/benpaodelulu_guajian/article/details/105901921

## 2. 提示time out的解决办法

reading in sources list data from /etc/ros/rosdep/sources.list.d

Hit http://github.com/ros/rosdistro/raw/master/rosdep/osx-homebrew.yaml

ERROR: unable to process source
[http://github.com/ros/rosdistro/raw/master/rosdep/base.yaml]:

\<urlopen error \_ssl.c:489: The handshake operation timed out\>
(http://github.com/ros/rosdistro/raw/master/rosdep/base.yaml)

ERROR: error loading sources list:

The read operation timed out

**解决方法：**

更改
/usr/lib/python2.7/dist-packages/rosdep2/下的三个文件sources_list.py、gbpdistro_support.py、rep3.py

中的DOWNLOAD_TIMEOUT = 15.0值，改大一点就可以了。

https://www.cnblogs.com/zxzmnh/p/11758103.html

## 3. rqt_plot运行报错

运行rqt_plot的时候，会出现以下报错，且rqt_plot的窗口不会显示网格，无法拖拽。

![clipboard.png](media/77b6a4dfddecf7eba653d2d305f75a18.png)

![clipboard.png](media/a88892e3bc1207116b0f4c8968819a79.png)

**解决方案**

在各种论坛、博客上找了好多解决办法，无奈都没有成功，最后误打误撞出了一个办法，终于算是能运行rqt_plot了...

在命令行输入：
``` bash
rqt
```

在加载出来的rqt窗口中，选择【Plugins】—【Visualization】—【Plot】，如你所见，rqt_plot运行成功！

https://blog.csdn.net/weixin_36965307/article/details/104948601

## 4. rospkg、rosdistro not found error

![clipboard.png](media/a1ae11549e97846ad4f8c398eb5de10a.png)

![clipboard.png](media/0746d79fdf9baa3aaec5e429e3453e88.png)

出现这个错误时，
``` bash
pkg_resources.DistributionNotFound: The 'rosdistro\>=0.7.5' distribution was not
found and is required by rosdep

pkg_resources.DistributionNotFound: The 'rospkg\>=1.1.10' distribution was not
found and is required by rosdep
```

安装如下两个包即可
``` bash
sudo apt-get install python-rospkg python-rosdistro
```
