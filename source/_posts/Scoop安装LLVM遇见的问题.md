---
title: Scoop安装LLVM遇见的问题
date: 2020-04-23 18:30:23
tags:
---

使用`scoop`安装`llvm`很简单：

``` shell 
scoop install llvm
```

不过安装过程中出现了一点问题，在结束阶段出现了这个：{% asset_img error.png %}
因此要安装msvc。

scoop中没有msvc的安装途径，不过我们可以通过chocolate来安装。

安装过程中发现本机的.NET Framework好像有些不正常，导致出现clr 8000405错误，但不知道是什么原因导致的（可能是老婆上次乱下东西给我装了一堆流氓软件的缘故吧- -！）。不过没关系，可以通过choco来强制重新安装.NET：
``` shell
choco install dotnetfx --force
```
之后就准备msvc的安装了。

首先安装vcbuildtool：

``` shell
choco install visualstudio2019buildtools
```
安装完成后，启动安装过程中被顺带安装上的微软官方程序`visual studio installer`。选择修复，之后选中Microsoft VC++支持安装就好了。

