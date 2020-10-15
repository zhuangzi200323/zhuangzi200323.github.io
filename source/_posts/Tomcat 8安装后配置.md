---
title: Tomcat 8安装后配置
date: 2019-06-02 15:06:15
img: '/medias/12.png'
categories:
- spring
tags:
- Tomcat
---

## Tomcat 8安装后配置

Tomcat 8安装完成，做如下配置后可使用下图红框中的功能

![](media/321a5e542c6cb67ff587ace737bb781d.jpeg)

### 1. 打开conf\\tomcat-users.xml文件，加入
``` bash
<role rolename="manager-gui"/>

<role rolename="manager-script"/>

<role rolename="manager-jmx"/>

<role rolename="manager-status"/>

<role rolename="manager-script"/>

<role rolename="admin-gui"/>

<role rolename="admin-script"/>

<user username="admin" password="123456"
roles="manager-gui,manager-script,manager-status,admin-gui"/>

<user username="root" password="123456"
roles="manager-script,manager-jmx,admin-script"/>
```

![](media/46095471e76dccab7b2b1426d067b9d1.jpeg)

### 2. 在conf\\Catalina\\localhost下新增manager.xml文件，内容如下：

```bash
<Context privileged="true" antiResourceLocking="false"
      docBase="${catalina.home}/webapps/manager">

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$"/>

</Context>
```

![](media/2a3f8e9509750e52be858927ac956dfe.jpeg)

安成上述两步后，红框中功能就可以访问了
