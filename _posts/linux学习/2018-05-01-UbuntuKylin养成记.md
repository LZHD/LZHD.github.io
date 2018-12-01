---
layout : life
title: UbuntuKylin养成记
category : linux学习
duoshuo: true
date : 2018-05-01
---

	作者 : LZHD
	日期 : 2018-05-01(于凌晨00:50)
	版本 : 0.0.2

<!-- more -->

### UbuntuKylin养成记

**1. 软件**

1. Shadowsocks(梯子)
2. teamviewer(远程)
3. virtualbox(虚拟机)
4. google-chrome(浏览器)
5. netease-cloud-music(网易云音乐1.0)
6. WebStorm(前端)
7. IdeaIU(后端)
8. WeChat(微信)
9. VisualStudio(微软编辑器)
10. AndroidStudio(APP)
11. Inkcape(矢量图)
12. JD-JUI(反编译)
13. Meld-Diff(文件对比)
14. JasperSoftStudio(报表)
15. Kodi(影音)
16. Terminator(超级终端)
17. geogebra-classic(几何画板)
18. Remarkable(markdown编辑器)
19. Vim(文本编辑器)
20. Apache2(服务器)
21. [git-diffall](https://github.com/thenigan/git-diffall)(git目录对比)
22. WebTorren(类似迅雷)
23. Privoxy(代理服务器)
24. [Scrcpy](https://github.com/Genymobile/scrcpy)(Android屏幕共享)
25. aptitude(Debian及其衍生系统中强大的包管理工具,类似apt-get,主要处理依赖问题)
26. MusicLake(音乐播放器)[手机端](https://github.com/caiyonglong/MusicLake)|[PC端](https://github.com/sunzongzheng/music)

**2. 常见问题**

>* 软件包依赖

```sh
    sudo apt-get update
    sudo apt-get install -f
   
```
>* 文件管理器卡死

```sh
    pkill nautilus
    
```
>* 搜狗中文输入法异常

```sh
    rm -rf ~/.config/SogouPY* ~/.config/sogou*

```