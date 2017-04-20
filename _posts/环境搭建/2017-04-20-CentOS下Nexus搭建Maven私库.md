---
layout : life
title: CentOS下Nexus搭建Maven私库
category : 环境搭建
duoshuo: true
date : 2017-04-20
---



```
	作者 : daodaoliang@yeah.net
	日期 : 2017-04-20
	版本 : 0.0.4
```

<!-- more -->

[TOC]

### 1. `Maven`私服简介

搭建私服可以做什么？
1、如果公司开发组的开发环境全部内网，这时如何连接到在互联网上的Maven中央仓库呢？
2、如果公司经常开发一些公共的组件，如何共享给各个开发组，使用拷贝方式吗？如果这样，公共库升级了怎么办？
当然可以解决的问题可能不止上面两点，下面来介绍在`CentOS`中搭建自己的`Maven`私服，使用`Nexus`。

### 2. `java` 运行环境准备

根据官网的介绍，运行环境最好是使用最新的8版本，官网文如下:

```
Nexus Repository Manager requires a Java 8 Runtime Environment (JRE) from Oracle. The distributions for OSX and Windows include suitable runtime environments for the specific operating system. The distributions for Unix do not include the runtime environment.
If you prefer to use an external runtime or use a Unix operating system, it is recommended to use the latest version of Java 8 available from the Oracle website. You can choose to install the full JDK or the JRE only.
```

#### 2.1 本地`java`版本确认

如果系统是新安装的并且没有进行相关配置的话，默认应该是`openJDK1.7版本`;

![java版本查看](/res/img/blog/环境搭建/java-version.png)

#### 2.2 安装新的`JDK`

* 由于原有系统很多包都是依赖与JDK，以防万一，我就没有卸载原有的JDK而是在安装一个8版本的JDK;
* 在[JDK官网][1]找到对应的`rpm`包进行下载并安装,建议本机迅雷下载完成后再传到服务器,也可以找一些速度快的镜像站进行下载;
* 下载完后直接安装，然后查看对应的版本信息是否正确;

![](/res/img/blog/环境搭建/java-upgrade.png)

* 顺手改一下`JAVA_HOME`的环境变量;

![java-home](/res/img/blog/环境搭建/java-home.png)

### 3. 安装 `Nexus`

#### 3.1 下载`Nexus`

* 从[官网下载][2]对应的安装包到服务器

**PS**：建议在本机用迅雷下载完了在传上去，或者用一些快一点镜像站；

```
	wget https://www.sonatype.com/oss-thank-you-tar.gz
```

#### 3.2 安装 `Nexus`

* 解压安装包

```
	tar zxvf oss-thank-you-tar.gz
```

* 安装成服务

```
	mv ./nexus-3.1.1-X86_64/  /usr/local/nexus3
```

增加环境变量`NEXUS_HOME`

![nexus环境变量](/res/img/blog/环境搭建/nexus.png)

创建并开启服务

```
	sudo ln -s $NEXUS_HOME/bin/nexus /etc/init.d/nexus
    cd /etc/init.d
    sudo chkconfig --add nexus
    sudo chkconfig --levels 345 nexus on
    sudo service nexus start
```

### 4. 配置 `Nexus`

#### 4.1 配置`WEB` 访问

* 开启 `8081` 端口

```
	vim /etc/sysconfig/iptables
```

增加如下一条:

![iptables](/res/img/blog/环境搭建/iptables.png)

* 浏览器访问

在浏览器中打开对应的URL 例如: [http://10.140.22.205:8081][3],默认管理员用户名和密码如下:

```
	user: admin
    pwd: admin123
```

![](/res/img/blog/环境搭建/maven.png)

默认`nexus`已经帮我们创建了五个库和两个组,我们可以直接使用,其中`Type`为`prosy`的为代理库,`Type`为`hosted`的为宿主库,`Type`为`group`的为组;

  一般用到的仓库种类是`hosted`、`proxy`。`Hosted`代表宿主仓库，用来发布一些第三方不允许的组件，比如`Oracle`驱动、比如商业软件`jar`包。`Proxy`代表代理远程的仓库，最典型的就是`Maven`官方中央仓库、`JBoss`仓库等等。如果构建的`Maven`项目本地仓库没有依赖包，那么就会去这个代理站点去下载，那么如果代理站点也没有此依赖包，就回去远程中央仓库下载依赖，这些中央仓库就是`proxy`。代理站点下载成功后再下载至本机。

#### 4.2 本地项目配置

在项目的`pom.xml`或者`settings.xml`文件里加入一下配置信息（区别，`pom.xml`是针对当前项目，`settings.xml`是全局的针对所有项目）配置信息中的`id`,`name`和`url`跟上图中的仓库对应，`type`为`proxy`，说明它只是代理，只能用于下载`jar`包，不能用于发布项目。

```xml
<repositories>
    <repository>
        <id>maven-central</id>
        <name>maven-central</name>
        <url>http://10.140.22.205:8081/repository/maven-central/</url>
        <layout>default</layout>
        <snapshotPolicy>always</snapshotPolicy>
         <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

其他使用可以[参考这里][4]

[1]:http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[2]:https://www.sonatype.com/download-oss-sonatype
[3]:http://10.140.22.205:8081
[4]:https://segmentfault.com/a/1190000005966312
