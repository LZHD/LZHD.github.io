---
layout : life
title: scrcpy配置(手机屏幕共享)
category : linux学习
duoshuo: true
date : 2018-05-01
---

	作者 : LZHD
	日期 : 2018-12-01
	版本 : 0.0.1

<!-- more -->

## scrcpy配置(手机屏幕共享)

### 1.前期准备[参考](https://github.com/Genymobile/scrcpy/blob/master/BUILD.md)

#### 以Ubuntu16.04为例

```bash
# runtime dependencies(编译依赖)
sudo apt install ffmpeg libsdl2-2.0.0
# client build dependencies
sudo apt install make gcc pkg-config meson ninja-build \
                 libavcodec-dev libavformat-dev libavutil-dev \
                 libsdl2-dev
sudo apt install python3-pip
pip3 install meson
```
***备注：***

* Ubuntu16.04安装libsdl2-dev的过程中可能会有依赖问题，将源切换到主服务器执行以下命令

```bash
sudo apt-get update
sudo apt-get install -f
sudo apt-get libsdl2-dev
```

### 2.开始编译

```bash
meson dir --buildtype release --strip -Db_lto=true \
    -Dprebuilt_server=/path/to/scrcpy-server.jar
cd dir
ninja
sudo ninja install
```
***备注：***

* 此操作在scrcpy源代码目录下进行
* dir为自己所创建的编译输出目录
* scrcpy-server.jar所在路径必须为系统绝对路径

### 3.实际效果

![实际效果](/res/img/blog/linux学习/2018-12-01%2012-32.gif)

