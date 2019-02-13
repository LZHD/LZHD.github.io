---
layout : life
title: UbuntuKylin养成记
category : linux学习
duoshuo: true
date : 2018-05-01
---

	作者 : LZHD
	日期 : 2018-05-01(于凌晨00:50)
	版本 : 0.0.3

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
8. [WeChat](https://github.com/geeeeeeeeek/electronic-wechat)(微信)
9. VisualStudioCode(微软编辑器)
10. AndroidStudio(APP)
11. [Inkcape](https://inkscape.org)(矢量图)
12. [JD-JUI](http://jd.benow.ca)(反编译)
13. Meld-Diff(文件对比)
14. [JasperSoftStudio](https://community.jaspersoft.com/project/jaspersoft-studio)(报表)
15. Kodi(影音)
16. Terminator(超级终端)
17. [geogebra-classic](https://www.geogebra.org)(几何画板)
18. [Remarkable](http://remarkableapp.github.io)(markdown编辑器)
19. Vim(文本编辑器)
20. Apache2(服务器)
21. [git-diffall](https://github.com/LZHD/git-diffall)(git目录对比)
22. [WebTorren](https://webtorrent.io)(类似迅雷)
23. [Privoxy](http://www.privoxy.org)(代理服务器)
24. [Scrcpy](https://github.com/Genymobile/scrcpy)(Android屏幕共享)
25. aptitude(Debian及其衍生系统中强大的包管理工具,类似apt-get,主要处理依赖问题)
26. ~~MusicLake(音乐播放器)~~[手机端](https://github.com/caiyonglong/MusicLake)---[PC端](https://github.com/sunzongzheng/music)
27. peek(gif录制)
28. [calibre](https://calibre-ebook.com/download_linux)(电子书阅读)
29. [Albert](https://github.com/albertlauncher/albert)(全局搜索扩展)
30. [Shutter](http://shutter-project.org)(截图)
31. Kazam(录屏)
32. Unity Tweak Tool(适用于ubuntu Unity桌面的设置管理器)
33. CompizConfig(设备管理器)
34. [xDroid](https://www.linzhuotech.com/index.php/home/index/down.html)(跨平台安卓模拟器)
35. [ProxyeeDown](https://github.com/proxyee-down-org/proxyee-down)(跨平台 HTTP 高速下载器)
36. Linux Game(SuperTux、SuperTuxKart、btanks、pingus)
37. gnome-web-photo(搭配Shutter网页截图)
38. [Pick](https://kryogenix.org/code/pick/)(拾色器)
39. [fluxgui](https://github.com/xflux-gui/fluxgui)(根据光线自动调节你的电脑屏幕显示)
40. Screen(多会话终端)
41. [有道词典](http://cidian.youdao.com/index-linux.html)(使用deepin版)
42. [Skype](https://www.skype.com/zh-Hans/get-skype/)(网络通讯工具)
43. [Xmonad](https://wiki.archlinux.org/index.php/Xmonad_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))(平铺式窗口管理器)
44. [Xrandr](https://wiki.archlinux.org/index.php/Xrandr_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))(多显示器配置工具)
45. [ARandR](https://christian.amsuess.com/tools/arandr/)(多显示器配置工具Xrandr的GUI工具)
46. [deepin-wine-ubuntu](https://github.com/wszqkzqk/deepin-wine-ubuntu)(借助此环境可以安装deepin下的常用的Windows软件，比如：微信、QQ等)
47. [draw.io](https://www.draw.io/)(图表软件，用于制作流程图，流程图，组织结构图，UML，ER和网络图)
48. sogoupinyin(搜狗输入法)
49. WPS(办公软件)

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