---
title: turtlebot3 gazebo仿真-基于ros melodic
date: 2020-05-22 15:06:15
img: '/medias/13.jpg'
categories:
- ROS
tags:
- turtlebot3
- gazebo
---

## turtlebot3 gazebo仿真-基于ros melodic

### turtlebot3 gazebo仿真

**依赖基础**

-   ros
    gazebo等，安装见<https://blog.csdn.net/weixin_41469272/article/details/105289174>

-   turtlebot3系列包
```bash
sudo apt install ros-melodic-turtlebot3\*
```
-   slam(这里使用gmapping)
```bash
sudo apt install ros-melodic-gmapping
```
-   navigation
```bash
sudo apt install ros-melodic-navigation
```

## 一、turtlrbot3 给定目标点，未知地图，自主定位建图及局部规划

### 1. 启动ros master
```bash
roscore
```
注意：打开新终端，运行turtlebot3相关包时，终端都需指定bot类型：即打开终端需执行：
```bash
export TURTLEBOT3_MODEL=burger
```
### 2. 启动turtlebot3 gazebo仿真环境
```bash
roslaunch turtlebot3_gazebo turtlebot3_house.launch
```
gazebo中显示turtlebot3及仿真环境

![在这里插入图片描述](media/fa6831b58446e2ec53931cc6155c10cb.png)

### 3. 启动rviz显示雷达数据

以下方式二选一，嫌麻烦选1，想了解rviz使用选2

1）直接打开写好的rviz配置文件：

roslaunch turtlebot3_gazebo turtlebot3_gazebo_rviz.launch

2）自己配置rviz

\*1. 终端输入rviz打开rviz界面

\*2. 将global option 下的fixed
frame改成odom，因为当前map坐标没有被创建，在执行slam的时候map坐标才会被创建

\*3. 点击下方add，添加laserscan ，将topic 设定为scan

此时会在界面中显示雷达点云，默认点云比较小，可以将size值改大一些，如0.05，点云数据会变大清晰。

\*4.
点击add，添加tf，则显示出多个坐标系，点击frame，去掉除base_link与odom其余的坐标系

### 4. 打开turtlebot3的键盘控制

roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch

按waxd等按键，观察rviz中base_link的移动是否一致。

### 5. 运行gmapping

rosrun gmapping slam_gmapping

### 6. rviz下add map，topic选择/map，rviz显示slam创建地图

![在这里插入图片描述](media/d4522f7f0e6d7572a54229186d4ef6f5.png)

### 7.  在打开的按键控制终端下，按waxd等按键，观察slam的创建地图的变换

### 8.  关闭turtlebot3的键盘控制

### 9.  打开move_base
```bash
roslaunch turtlebot3_navigation move_base.launch
```
### 10.  rviz中add path，topic选择 global path

### 11.  点击2d nav_goal，在rviz显示的地图上，用鼠标点击选择一个目标点，机器人自行规划路径运行至目标点

### 12.  最后可以把新配置的rviz保存，这样下次不用再挨个add topic

![在这里插入图片描述](media/671979538d2804e0cdcfec2a7ee74555.png)

## 二、 创建地图并保存，根据已知地图导航，全局规划

**slam建图并保存**

首先打开ros master：
``` bash
roscore
```
注意：打开新终端，运行turtlebot3相关包时，终端都需指定bot类型：即打开终端需执行：

export TURTLEBOT3_MODEL=burger

### 1.  启动turtlebot3 gazebo仿真环境
```bash
roslaunch turtlebot3_gazebo turtlebot3_house.launch
```
### 2.  打开新终端，启动slam
```bash
roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```
### 3.  新终端，启动键盘控制程序
```bash
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```
### 4.  利用按键aw等移动机器人，使地图完整，保存地图到主\~目录下
```bash
rosrun map_server map_saver -f \~/map
```
### 5.  关闭turtlebot3_teleop，在turtlebot3_teleop运行终端下，ctrl+c就行

### 6.  运行turtlebot3_navigation
```bash
roslaunch turtlebot3_navigation turtlebot3_navigation.launch
map_file:=\$HOME/map.yaml
```
**参考连接**

<https://www.ncnynl.com/archives/201707/1787.html>

**转自**
<https://blog.csdn.net/weixin_41469272/article/details/106139306>
