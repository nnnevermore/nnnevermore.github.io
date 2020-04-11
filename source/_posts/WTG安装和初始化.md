---
title: WTG安装和初始化
date: 2020-04-08 16:04:26
tags: 
    - 'WTG'
    - 'Windows'
---

[WTG（windows to go）](https://docs.microsoft.com/en-us/windows/deployment/planning/windows-to-go-overview)是什么，想必不用多做介绍。下面是安装和配置的过程。

WTG的制作过程，有很多种方案：

1. 官方工具
2. Rufus
3. WTGA（WTG助手，国产开源）
4. AOMEI Partition Assistant

具体用法就不赘述了，自行google即可。其中，1和2只有在制作WTG的宿主系统（也就是本机的系统）为windows10才有效，3和4都支持在win7上制作更高版本的WTG系统。不过有一点需要注意：制作过程中需要windows镜像中的install.wim文件（在source目录）。但是，如果使用[Microsoft官方工具](https://www.microsoft.com/zh-cn/software-download/windows10)（运行工具之后有直接下载iso文件的选项）下载的iso文件的话，目前的最新版本是没有install.wim了，取而代之的是一个install.esd文件，而WTGA虽然号称支持esd文件，但是实际使用过程中却造成失败。AOMEI则只支持install.wim。所以必须的已不是将esd文件转换为wim文件，这一步可以通过[dism++](https://www.chuyu.me/zh-Hans/)可以很方便转换。

## 初始化设置

安装好系统后，配置一下，使其更加易用。

### `shadowsocks`
客户端备份: {% asset_link ShadowsocksR-win-4.9.1.zip %}

配置好后记得开启本地http代理

### [`utools`](https://www.u.tools/)
方便易用，风头正劲的新型软件。

### [`scoop`](https://scoop.sh/)
安装步骤：

1. 打开powershell
2. 执行 `Set-ExecutionPolicy RemoteSigned -scope CurrentUser`
3. 执行 `Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')` 或者简短版本 `iwr -useb get.scoop.sh | iex`.

安装完scoop后，首先安装两个包：`7zip`和`git`, scoop会自动使用系统代理，配置了ss的话，速度应该比较正常。
git安装好后，设置代理：
```shell
git config --global http.proxy 'http://127.0.0.1:1080'
git config --global https.proxy 'https://127.0.0.1:1080'
```
或sock5代理：
```shell
git config --global http.proxy 'sock5://127.0.0.1:1080'
git config --global https.proxy 'sock5://127.0.0.1:1080'
```
之后可以添加几个bucket:
```shell
bear            (scoop bucket add bear https://github.com/AStupidBear/scoop-bear)                             
extras                                       
java                                         
main                                         
TheRandomLabs   (scoop bucket add TheRandomLabs https://github.com/TheRandomLabs/Scoop-Bucket.git)                            
versions                                     
```

之后，安装其他工具，如果安装了aria2的话，scoop会调用它来下载，具体可以配置，目前已安装的包列表如下：
```powershell
PS C:\windows\system32> scoop list         
Installed apps:                            
                                           
  7zip 19.00                               
  aria2 1.35.0-1                           
  aria-ng-gui 3.1.0 [extras]               
  conemu 19.10.12 [extras]                 
  conemu-color-themes ayu-theme [extras]   
  diskgenius 5.2.0.884 [extras]            
  dismplusplus 10.1.1001.10 [extras]       
  everything 1.4.1.969 [extras]            
  ffmpeg 4.2.2                             
  firefox 75.0 [extras]                    
  frp 0.32.1                               
  git 2.26.0.windows.1                     
  git-lfs 2.10.0                           
  googlechrome 80.0.3987.163 [extras]      
  hexo-blog-client 1.2.9 [extras]          
  listen1desktop 2.5.1 [extras]            
  miniconda3 4.7.12.1 [extras]             
  mobaxterm 20.1 [extras]                  
  neovim 0.4.3                             
  nmap 7.80                                
  nodejs 13.12.0                           
  officetoolplus 7.5.0.3 [h404bi_dorado]   
  potplayer 200317 [extras]                
  qttabbar 1038 [bear]                     
  steam nightly-20200408 [extras]          
  sublime-text 3.2.2-3211 [extras]         
  sudo 0.2020.01.26                        
  teamviewer 15.4.4445 [extras]            
  Telegram 2.0.1 [extras]                  
  update-qttabbar 1040 [bear]              
  vcredist2010 10.0.40219 [extras]         
  vcredist2019 14.24.28127.4 [extras]      
  vlc 3.0.8 [extras]                       
  vscode-insiders nightly-20200408 [extras]
  wireshark 3.2.2 [extras]                 
  youtube-dl 2020.03.24                    
  youtube-dl-gui 0.4 [extras]                        
```

最好用sudo来执行安装。

scoop安装的包有时候本该加入右键菜单的却没有自动加，而且不像vscode安装完还有提示该执行某个路径下的reg。所以需要手动加入，这里加入一些常用的：
```shell
How to

Save the code below as foobar.reg and execute it.
Notice ! Modify path !

    Modify the path to your path! For instance: C:\\Users\\foobar\\scoop\\apps\\git\\current\\git-bash.exe to \\you\\dope\\path\\sth.exe.
    Save the content as .reg file and execute it as administrator.

Install and Uninstall Context
Sublime-text

Install:

Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Classes\*\shell\Open with &Sublime]
@="Open with &Sublime"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe"
[HKEY_CURRENT_USER\Software\Classes\*\shell\Open with &Sublime\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe\" \"%1\""

[HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with &Sublime]
@="Open with &Sublime"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe"
[HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with &Sublime\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe\" \"%1\""

[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with &Sublime]
@="Open with &Sublime"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe"
[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with &Sublime\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\sublime-text\\3211\\sublime_text.exe\" \"%V\""

Uninstall:

Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with &Sublime]
[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with &Sublime\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with &Sublime]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with &Sublime\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with &Sublime]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with &Sublime\command]

emacs

Install:

Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Classes\*\shell\Open with emacs]
@="Open with emacs"
"Icon"="C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe"
[HKEY_CURRENT_USER\Software\Classes\*\shell\Open with emacs\command]
@="\"C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe\" \"%1\""

[HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with emacs]
@="Open with emacs"
"Icon"="C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe"
[HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with emacs\command]
@="\"C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe\" \"%1\""

[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with emacs]
@="Open with emacs"
"Icon"="C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe"
[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with emacs\command]
@="\"C:\\Users\\foobar\\scoop\\apps\\emacs\\current\\bin\\runemacs.exe\" \"%V\""

Uninstall:

Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with emacs]
[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with emacs\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with emacs]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with emacs\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with emacs]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with emacs\command]

Vim

Install:

Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\nvim]
@="Open With NVIM"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe"

[HKEY_CLASSES_ROOT\*\shell\nvim\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe\"  \"%1\""

[HKEY_CLASSES_ROOT\Directory\Background\shell\nvim]
@="Open With NVIM"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\nvim\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe\"  \"%V\""


[HKEY_CLASSES_ROOT\Directory\shell\nvim]
@="Open With NVIM"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe"

[HKEY_CLASSES_ROOT\Directory\shell\nvim\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\neovim\\current\\bin\\nvim-qt.exe\"  \"%1\""

Unistall:

Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with NVIM]
[-HKEY_CURRENT_USER\Software\Classes\*\shell\Open with NVIM\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with NVIM]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\Open with NVIM\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with NVIM]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\Open with NVIM\command]

Git bash

Inistall:

Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\git_shell]
@="Git Ba&sh Here"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\git\\current\\git-bash.exe"

[HKEY_CLASSES_ROOT\Directory\shell\git_shell\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\git\\current\\git-bash.exe\" \"--cd=%1\""

[HKEY_CLASSES_ROOT\Directory\Background\shell\git_shell]
@="Git Ba&sh Here"
"Icon"="C:\\Users\\Notebook\\scoop\\apps\\git\\current\\git-bash.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\git_shell\command]
@="\"C:\\Users\\Notebook\\scoop\\apps\\git\\current\\git-bash.exe\" \"--cd=%v.\""

Unistall:

Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\*\shell\git_bash]
[-HKEY_CURRENT_USER\Software\Classes\*\shell\git_bash\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\git_bash]
[-HKEY_CURRENT_USER\Software\Classes\Directory\shell\git_bash\command]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\git_bash]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\git_bash\command]

Difference

Method 1 you need to edit via regedit to change the value to your git-bash path.

Method 2 you just edit here and run it.

EOF
```

有一个有用的网站可以检索scoop的包：[Scoop App](https://scoop.if-she.com/)

最后可以安装[chocolatey](https://chocolatey.org/install)来和scoop互相补充:
管理员权限的powershell中执行：
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```
