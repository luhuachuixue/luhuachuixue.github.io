---
title: "Win10开发环境搭建"
date: "2021-01-01T19:55:56+08:00"
slug: 37386fca
tags: ["Windows"]
categories: ["开发"]
draft: false
---

## 常用软件

```
7-Zip VSCode Chrome Firefox IDM 迅雷 百度网盘 iTunes QQ 网易云音乐 洛雪音乐助手 萤石云 TeamViewer Office Calibre VLC OBS Adobe
格式工厂 DiskGenius carnac Snipaste QTranslate TrafficMonitor MathJax Graphviz PlantUML Pandoc wkhtmltopdf Telegram Clash
MinGW-w64 JDK Python Lua Go Perl CodeBlocks VS Qt PyCharm IDEA Android-Studio Source-Insight BCompare WinSCP MobaXterm
Navicat VMware Wireshark Git TortoiseGit SVN TortoiseSVN CMake Ninja Cppcheck Ant Tomcat Typora VNote
```

## 1 安装配置GCC

### 1.1 说明

**MinGW**`(Minimalist GNU for Windows)`分为较早开发的`MinGW32`和之后为编译64位程序开发的`MinGW-w64`

**MSYS**`(Minimal System)`分为早期开发的`MSYS`(已弃用)和现在流行的`MSYS2`

| MinGW/MSYS | 说明 |
| ---- | ---- |
| MinGW32  |  只能编译32位的程序 |
| MinGW-w64  |  不仅能编译64位程序，也能编译32位程序，还能进行交叉编译，即在32位主机上编译64位程序，在64位主机上编译32位程序 |
| mingw64  |  不依赖msys2的dll，直接运行 |
| msys64  |  依赖msys2的dll，才能运行 |

### 1.2 下载地址

- [MinGW](https://osdn.net/projects/mingw/releases/)

- [MinGW-w64](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release)

- [MinGW-w64-WinLibs](https://github.com/brechtsanders/winlibs_mingw/releases)

- [MinGW-w64-MCF](https://gcc-mcf.lhmouse.com)

- [GCC-TDM](https://github.com/jmeubank/tdm-gcc/releases)

### 1.3 解压

复制一份`mingw32-make.exe`，重命名为`make.exe`

### 1.4 环境变量`PATH`中添加

```
D:\Library\mingw64\bin
```

### 1.5 IDE配置

- compiler

```
D:/Library/mingw64
```

- include

```
D:/Library/mingw64/include
D:/Library/mingw64/include/c++/10.2.0
D:/Library/mingw64/x86_64-w64-mingw32/include
D:/Library/mingw64/lib/gcc/x86_64-w64-mingw32/10.2.0/include
```

- lib

```
D:/Library/mingw64/lib
D:/Library/mingw64/x86_64-w64-mingw32/lib
```

## 2 配置Git、Vim、Neovim、7-Zip

### 2.1 Git配置文件

| Git配置文件 | 路径 |
| ---- | ---- |
| Git全局配置文件  |  ${USERPROFILE}/.gitconfig <br> %USERPROFILE%/.gitconfig |
| Git系统配置文件  |  "${PROGRAMFILES}/Git/etc/gitconfig" <br> "%PROGRAMFILES%/Git/etc/gitconfig" |
| GitBash配置文件  |  "${PROGRAMFILES}/Git/etc/profile" <br> "%PROGRAMFILES%/Git/etc/profile" |
| GitBash提示符配置文件  |  "${PROGRAMFILES}/Git/etc/profile.d/git-prompt.sh" <br> "%PROGRAMFILES%/Git/etc/profile.d/git-prompt.sh" |
| GitBash主题配置文件  |  ${USERPROFILE}/.minttyrc <br> %USERPROFILE%/.minttyrc |

### 2.2 修改GitBash主题

```shell
vim ${USERPROFILE}/.minttyrc
	ThemeFile=dracula
	Font=Source Code Pro SemiBold
	FontHeight=18
	FontWeight=400
	FontSmoothing=full
	BoldAsFont=yes
	Transparency=low
	OpaqueWhenFocused=no
	CursorType=line
	Charset=UTF-8
	Locale=C
	Language=zh_CN
	Term=xterm-256color
	BellTaskbar=no
	Rows=20
	Columns=80
	ScrollbackLines=999999999
```

### 2.3 修改GitBash配置(管理员GitBash)

```shell
vim /etc/profile
	export LANG=C.UTF-8
	PROMPT_COMMAND='history -a'
	chcp.com 65001 > /dev/null
	alias ep='export all_proxy=socks5://127.0.0.1:7891 && git config --global http.proxy "socks5://127.0.0.1:7891" && git config --global https.proxy "socks5://127.0.0.1:7891"'
	alias epc='unset all_proxy && git config --global --unset http.proxy && git config --global --unset https.proxy'
	alias la='ls -A'
	alias l='ls -CF'
	alias ll='ls -alF'
	alias lll='ls -AlhF --sort=size . | tr -s " " | cut -d " " -f 5,9'
	alias ssh1='ssh chuixue@192.168.0.123 -p22'
	alias vmake='cmake -G"MinGW Makefiles" -S. -Bbuild && cd build && mingw32-make && cd ..'
	alias vrun='build/*.exe'
	alias vrm='rm -rf build'
```

### 2.4 修改GitBash提示信息PS1(管理员GitBash)

```shell
vim /etc/profile.d/git-prompt.sh
注释掉
	# PS1='\[\033]0;$TITLEPREFIX:$PWD\007\]' # set window title
	# PS1="$PS1"'\n'                 # new line
	# PS1="$PS1"'\[\033[32m\]'       # change to green
	# PS1="$PS1"'\u@\h '             # user@host<space>
	# PS1="$PS1"'\[\033[35m\]'       # change to purple
	# PS1="$PS1"'$MSYSTEM '          # show MSYSTEM
	# PS1="$PS1"'\[\033[33m\]'       # change to yellow
	# PS1="$PS1"'\w'                 # current working directory

	# PS1="$PS1"'\[\033[36m\]'  # change color to cyan

	# PS1="$PS1"'\[\033[0m\]'        # change color
	# PS1="$PS1"'\n'                 # new line
	# PS1="$PS1"'$ '                 # prompt: always $
添加
	PS1='\[\033]0;GitBash\007\]'   # set window title
	PS1="$PS1"                     # new line
	PS1="$PS1"'\[\033[32;1m\]'     # change to highlight green
	PS1="$PS1"'\u '                # user<space>
	PS1="$PS1"'\[\033[33;1m\]'     # change to highlight yellow
	PS1="$PS1"'\w'                 # current working directory

	PS1="$PS1"'\[\033[31m\]'  # change color to red

	PS1="$PS1"'\[\033[32;1m\]'     # change color
	PS1="$PS1"' ➜ '                # prompt: always ➜
	PS1="$PS1"'\[\033[36m\] '      # change color to cyan
```

### 2.5 GitBash快捷键

#### 2.5.1 添加快捷键

	C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Git
	右键 --> 快捷方式 --> 快捷键 --> Ctrl+Alt+T

#### 2.5.2 快捷键打开卡顿3秒

	Win+R --> services.msc --> SysMain --> 禁用
	重启Windows
	SysMain主要是用于机械硬盘的缓存，对固态没什么用，很占内存
		>=1809  SysMain
		<1809   superfetch
		XP      Prefetch

### 2.6 修改Git配置

#### 2.6.1 配置用户名、邮箱

```shell
git config --global user.name luhuachuixue && git config --global user.email luhuachuixue@gmail.com
```

#### 2.6.2 修改默认分支(2.28版本开始支持)

```shell
git config --global init.defaultBranch main
```

### 2.7 查看Git配置

#### 2.7.1 查看所有配置

```shell
git config -l
```

#### 2.7.2 查看全局配置(当前系统用户)

对应`${USERPROFILE}/.gitconfig`

```shell
git config --global -l
```

#### 2.7.3 查看系统配置(所有系统用户)

对应`"${PROGRAMFILES}/Git/etc/gitconfig"`

```shell
git config --system -l
```

### 2.8 GitBash的`PATH`与Windows的`PATH`对比

CMD输出`PATH`

```bat
echo %PATH%
```

GitBash输出`PATH`

```shell
echo $PATH
```

| 变动位置 | 变动内容 |
| ---- | ---- |
| 首部添加  |  /c/Users/haoran/bin <br> /mingw64/bin <br> /usr/local/bin <br> /usr/bin <br> /bin |
| 尾部添加  |  /usr/bin/vendor_perl <br> /usr/bin/core_perl |

### 2.9 添加实用终端程序

#### 2.9.1 下载地址

- [wget](https://eternallybored.org/misc/wget)

- [tree](https://sourceforge.net/projects/gnuwin32/files/tree)

- [rename](https://sourceforge.net/projects/rename/files)

#### 2.9.2 移动到指定目录

```shell
cp -r D:/Software/Development-Tools/GNU-Tool D:/Library/Tool
```

### 2.10 修改Vim配置(管理员GitBash)

```shell
vim /etc/vimrc
	set tabstop=4
	set expandtab
	set autoindent
	set nu
```

### 2.11 Neovim

#### 2.11.1 解压

#### 2.11.2 配置

参考D:/Software/Editor/Vim/Vim相关.txt

### 2.12 环境变量`PATH`中添加

```
C:\Program Files\Git\cmd
C:\Program Files\7-Zip
D:\Library\Tool
D:\Library\Neovim\bin
```

## 3 安装配置Golang

### 3.1 解压

### 3.2 配置环境变量

- 添加环境变量

```
GOROOT = D:\Library\go
GOPATH = D:\Projects\Go
GOBIN = %GOPATH%\bin
```

- `PATH`中添加

```
%GOBIN%
%GOPATH%
%GOROOT%\bin
```

### 3.3 配置文件

#### 3.3.1 查看配置文件

`${APPDATA}/go/env` 或 `%APPDATA%/go/env`

```shell
go env GOENV
```

#### 3.3.2 修改配置文件

```shell
go env -w xxx=xxx
```

### 3.4 配置代理

#### 3.4.1 代理服务器

- [proxy.golang.org](https://proxy.golang.org)

- [goproxy.cn](https://goproxy.cn)

- [goproxy.io](https://goproxy.io)

#### 3.4.2 默认

```
GO111MODULE=
GOPROXY=https://proxy.golang.org,direct
GOPRIVATE=
```

#### 3.4.3 修改

```shell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

#### 3.4.4 私有库不走代理

```shell
go env -w GOPRIVATE=*.corp.example.com
```

### 3.5 测试

```shell
go version
go env
```

## 4 安装配置Lua、Perl

### 4.1 Lua

[编译脚本下载地址](https://github.com/luhuachuixue/build-script)

#### 4.1.1 MSVC(5秒)

- 修改脚本build.cmd，放在源码压缩包同级目录

| 变量 | 说明 |
| ---- | ---- |
| lua_ver_major	 |  对应Lua主版本号 |
| lua_ver_minor  | 	对应Lua次版本号 |
| lua_ver_patch	 |  对应Lua补丁版本号 |
| lua_install_dir  |  对应Lua二进制存放目录 |
| msvc_env  |  对应MSVC环境配置BAT脚本路径 |

- 双击脚本build.cmd

#### 4.1.2 MinGW(5秒)

- 修改脚本build.cmd，放在源码压缩包同级目录

| 变量 | 说明 |
| ---- | ---- |
| lua_version  |  对应Lua版本 |
| lua_install_dir  |  对应Lua二进制存放目录 |

- 双击脚本build.cmd

### 4.2 Perl

#### 4.2.1 解压

#### 4.2.2 GitBash使用Strawberry Perl

```shell
export PATH=/d/Library/perl/perl/bin:/d/Library/perl/perl/site/bin:$PATH
export PERL5LIB=/d/Library/perl/perl/vendor/lib:/d/Library/perl/perl/site/lib
```

#### 4.2.3 官方运行环境BAT脚本

```bat
call D:\Library\perl\portableshell.bat
```

#### 4.2.4 测试

```shell
perl -v
perl -MCPAN -e shell
```

#### 4.3 环境变量`PATH`中添加

```
D:\Library\lua\bin
D:\Library\perl\perl\bin
D:\Library\perl\perl\site\bin
```

## 5 安装配置JDK

### 5.1 安装JDK

解压JDK和JavaFX的docs文档到JDK根目录

### 5.2 配置环境变量

#### 5.2.1 JDK8以下

JDK1.5以上，可不用设置CLASSPATH环境变量

| jar包 | 说明 |
| ---- | ---- |
| dt.jar  |  关于运行环境的类库，主要是swing的包 |
| tools.jar  |  关于一些工具的类库 |
| rt.jar  |  包含了jdk的基础类库，也就是你在java doc里面看到的所有类的class文件 |

- 添加环境变量

```
JAVA_HOME = C:\Program Files\Java\jdk1.8.0_271
CLASSPATH = .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
```

- `PATH`中添加

```
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

#### 5.2.2 JDK9以上

- 添加环境变量

```
JAVA_HOME = C:\Program Files\Java\jdk-15.0.1
CLASSPATH = .;%JAVA_HOME%\lib
```

- `PATH`中添加

```
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

#### 5.2.3 JDK9以上没有JRE(模块化)

管理员CMD/PowerShell

```bat
cd /D %JAVA_HOME%
bin/jlink.exe --module-path jmods --add-modules java.desktop --output jre
```

### 5.3 查看JDK源码

#### 5.3.1 NetBeans

`工具 -> Java平台 -> 源 -> "%JAVA_HOME%/src.zip"`

#### 5.3.2 Eclipse

`Window -> Preferences -> Java -> Installed JREs -> Edit -> rt.jar -> Source Attachment -> Extern Location -> "%JAVA_HOME%/src.zip"`

#### 5.3.3 IDEA

- `File -> Project Structure -> SDKs -> Sourcepath -> "%JAVA_HOME%/src.zip"`

- `File -> Project Structure -> SDKs -> Sourcepath -> "%JAVA_HOME%/javafx-src.zip"`

- `File -> Project Structure -> SDKs -> Documentation Paths -> "%JAVA_HOME%/docs/api"`

- `File -> Project Structure -> SDKs -> Documentation Paths -> "%JAVA_HOME%/api"`

### 5.4 修改JDK字体(管理员GitBash)

解决NetBeans的Consolas字体乱码

将`%JAVA_HOME%/jre/lib`目录下的`fontconfig.properties.src`文件，另存为`fontconfig.properties`文件

```shell
cd "${JAVA_HOME}/jre/lib"
cp fontconfig.properties.src fontconfig.properties
nvim fontconfig.properties
在Font File Names下面，新增Consolas字体
	filename.Consolas=CONSOLA.TTF
	filename.Consolas_Bold=CONSOLAB.TTF
	filename.Consolas_Italic=CONSOLAI.TTF
	filename.Consolas_Bold_Italic=CONSOLAZ.TTF
在Component Font Mappings下面，修改dialoginput字体的映射项为Consolas对应项
	dialoginput.plain.alphabetic=Consolas
	dialoginput.bold.alphabetic=Consolas Bold
	dialoginput.italic.alphabetic=Consolas Italic
	dialoginput.bolditalic.alphabetic=Consolas Bold Italic
```

### 5.5 升级JDK

- 卸载旧JDK，安装新JDK

- 修改`${NetBeans安装目录}/etc/netbeans.conf`的`netbeans_jdkhome`为新版本的路径

```
netbeans_jdkhome="C:/Program Files/Java/jdk-xxx"
```

- 重新修改一遍JDK的字体

## 6 安装配置Android

### 6.1 配置环境变量

- 添加环境变量(ANDROID_HOME被官方反对，不推荐使用)

```
ANDROID_SDK_ROOT = %LOCALAPPDATA%\Android\Sdk
```

- `PATH`中添加

```
%ANDROID_SDK_ROOT%\tools
%ANDROID_SDK_ROOT%\platform-tools
```

### 6.2 Android Studio Design界面不显示控件

`Project -> res -> values -> styles.xml`，Theme前面加一个Base

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
<style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
```

## 7 安装配置Python

### 7.1 安装Python

### 7.2 环境变量`PATH`中添加(自动添加)

```
C:\Python38
C:\Python38\Scripts
```

### 7.3 配置pip

#### 7.3.1 换源(GitBash)

注意：此文件必须是ANSI编码

```shell
mkdir ${USERPROFILE}/pip
nvim ${USERPROFILE}/pip/pip.ini
	#清华源，每隔5分钟同步一次
	[global]
	index-url = https://pypi.tuna.tsinghua.edu.cn/simple
	[install]
	trusted-host =  pypi.tuna.tsinghua.edu.cn
```

#### 7.3.2 升级pip(管理员CMD/PowerShell)

```bat
pip install -U pip
```

#### 7.3.3 安装第三方包

```bat
pip install numpy Pillow urllib3 requests selenium setuptools
```

#### 7.3.4 升级第三方包

```bat
pip install -U xxx
```

## 8 配置VSCode

### 8.1 全局设置

#### 8.1.1 全局配置文件

参考D:/Software/Editor/VS Code/Config/Config-Win.txt

`Ctrl+,` 打开设置

`${APPDATA}/Code/User/settings.json` 或 `%APPDATA%/Code/User/settings.json`

#### 8.1.2 集成终端使用Cmder

```json
"terminal.integrated.shell.windows": "cmd.exe",
"terminal.integrated.shellArgs.windows": ["/K", "D:/Library/cmder/vendor/init.bat"],
```

#### 8.1.3 集成终端使用MSYS

```json
"terminal.integrated.shell.windows": "D:/Library/msys64/msys2_shell.cmd",
"terminal.integrated.shellArgs.windows": ["-defterm", "-here", "-no-start", "-mingw64"],
```

### 8.2 VSCode预定义变量

参考[VSCode-DOC](https://code.visualstudio.com/docs/editor/variables-reference)

| 变量 | 说明 |
| ---- | ---- |
| ${workspaceFolder}  |  the path of the folder opened in VS Code |
| ${workspaceFolderBasename}  |  the name of the folder opened in VS Code without any slashes (/) |
| ${file}  |  the current opened file |
| ${relativeFile}  |  the current opened file relative to workspaceFolder |
| ${relativeFileDirname}  |  the current opened file's dirname relative to workspaceFolder |
| ${fileBasename}  |  the current opened file's basename |
| ${fileBasenameNoExtension}  |  the current opened file's basename with no file extension |
| ${fileDirname}  |  the current opened file's dirname |
| ${fileExtname}  |  the current opened file's extension |
| ${cwd}  |  the task runner's current working directory on startup |
| ${lineNumber}  |  the current selected line number in the active file |
| ${selectedText}  |  the current selected text in the active file |
| ${execPath}  |  the path to the running VS Code executable |
| ${defaultBuildTask}  |  the name of the default build task |

### 8.3 配置C++开发环境

参考[VSCode-DOC](https://code.visualstudio.com/docs/cpp/launch-json-reference)

在工作区内创建目录.vscode，目录中新建3个JSON文件

| 文件 | 说明 |
| ---- | ---- |
| c_cpp_properties.json  |  指定编译器路径 <br> 参考D:/Software/Editor/VS Code/C-C++/c_cpp_properties.json |
| tasks.json  |  指定如何构建可执行文件 <br> 参考D:/Software/Editor/VS Code/C-C++/tasks.json |
| launch.json  |  指定调试器设置 <br> 参考D:/Software/Editor/VS Code/C-C++/launch.json |

### 8.4 自定义代码模板

#### 8.4.1 自定义C模板

参考D:/Software/Editor/VS Code/Config/snippet/c.json

`Ctrl + Shift + P` --> `Configure User Snippet` --> `c.json`

`${APPDATA}/Code/User/snippets/c.json` 或 `%APPDATA%/Code/User/snippets/c.json`

#### 8.4.2 自定义C++模板

参考D:/Software/Editor/VS Code/Config/snippet/cpp.json

`Ctrl + Shift + P` --> `Configure User Snippet` --> `cpp.json`

`${APPDATA}/Code/User/snippets/cpp.json` 或 `%APPDATA%/Code/User/snippets/cpp.json`

#### 8.4.3 自定义Java模板

参考D:/Software/Editor/VS Code/Config/snippet/java.json

`Ctrl + Shift + P` --> `Configure User Snippet` --> `java.json`

`${APPDATA}/Code/User/snippets/java.json` 或 `%APPDATA%/Code/User/snippets/java.json`

#### 8.4.4 自定义CMake模板

参考D:/Software/Editor/VS Code/Config/snippet/cmake.json

`Ctrl + Shift + P` --> `Configure User Snippet` --> `cmake.json`

`${APPDATA}/Code/User/snippets/cmake.json` 或 `%APPDATA%/Code/User/snippets/cmake.json`

### 8.5 GitBash+CMake+MinGW运行

- 分别取别名vmake、vrun、vrm

```shell
alias vmake='cmake -G"MinGW Makefiles" -S. -Bbuild && cd build && mingw32-make && cd ..'
alias vrm='rm -rf build'
alias vrun='build/*.exe'
```

- 编译执行

```shell
vmake
vrun
vrm
```

### 8.6 CMake调试

- tasks.json

```json
{
	"tasks": [
		{
			"type": "shell",
			"label": "make",
			"command": "make",
			"args": [],
			"options": {
				"cwd": "${workspaceFolder}/build"
			},
			"group": {
				"kind": "build",
				"isDefault": true
			}
		}
	],
	"version": "2.0.0"
}
```

- launch.json

```json
"program": "${workspaceFolder}/build/${workspaceFolderBasename}.exe",
"preLaunchTask": "make"
```

### 8.7 SSH过程试图写入的管道不存在

#### 8.7.1 原因

本地的known_hosts文件记录服务器信息与实际服务器的信息冲突了

#### 8.7.2 删除主机对应IP的信息

`${USERPROFILE}/.ssh/known_hosts` 或 `%USERPROFILE%/.ssh/known_hosts`

## 9 安装配置CUDA、cuDNN

### 9.1 下载地址

- [NVIDIA Driver](https://www.nvidia.cn/geforce/drivers/)

- [NVIDIA CUDA](https://developer.nvidia.com/zh-cn/cuda-downloads)

- [NVIDIA cuDNN](https://developer.nvidia.com/rdp/cudnn-download)

- [NVIDIA Video Codec SDK](https://developer.nvidia.com/nvidia-video-codec-sdk/download)

### 9.2 更新显卡驱动

### 9.3 安装配置CUDA

#### 9.3.1 版本兼容性

参考D:/Library/开发环境搭建/CUDA相关.txt

#### 9.3.2 配置环境变量

- 自动添加环境变量

```
CUDA_PATH = C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1
CUDA_PATH_V11.1 = C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1
NVCUDASAMPLES_ROOT = D:\Library\CUDA\samples
NVCUDASAMPLES11_1_ROOT = D:\Library\CUDA\samples
```

- 自动在`PATH`中添加

```
%CUDA_PATH%\bin
%CUDA_PATH%\libnvvp
C:\Program Files\NVIDIA Corporation\Nsight Compute 2020.1
```

- 手动添加环境变量

```
CUDA_LIB_PATH = %CUDA_PATH%\lib\x64
CUDA_SDK_BIN_PATH = %NVCUDASAMPLES_ROOT%\bin\win64\Release
CUDA_SDK_LIB_PATH = %NVCUDASAMPLES_ROOT%\common\lib\x64
```

### 9.4 配置cuDNN

- 将bin目录下所有文件复制到%CUDA_PATH%\bin目录下

- 将include目录下所有文件复制到%CUDA_PATH%\include目录下

- 将Lib\x64目录下所有文件复制到%CUDA_PATH%\lib\x64目录下

### 9.5 配置NVIDIA Video Codec SDK

- 将Interface目录下所有文件复制到%CUDA_PATH%\include目录下

- 将Lib\x64目录下所有文件复制到%CUDA_PATH%\lib\x64目录下

- 将Lib\Win32目录下所有文件复制到%CUDA_PATH%\lib\Win32目录下

### 9.6 配置VS2017+CUDA开发环境

#### 9.6.1 新建win32应用程序

#### 9.6.2 配置X64库

- `视图 -> 属性管理器`

- 双击`Debug|x64->Microsoft.Cpp.x64.User`

- `通用属性 --> C/C++ --> 常规 --> 附加包含目录`添加

	C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1\include

- `通用属性 --> 链接器 --> 常规 --> 附加库目录`添加

	C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1\lib\x64

- `通用属性 --> CUDA C/C++ --> Common --> CUDA Toolkit Custom Dir`添加

	C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1

- `通用属性 --> 链接器 --> 输入 --> 附加依赖项`添加

	- cuda.lib

	- cudadevrt.lib

	- cudart.lib

	- cudart_static.lib

	- cudnn.lib

	- OpenCL.lib

#### 9.6.3 配置X86库

- `视图 -> 属性管理器`

- 双击`Debug|Win32->Microsoft.Cpp.Win32.User`

- `通用属性 --> C/C++ --> 常规 --> 附加包含目录`添加

    C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1\include

- `通用属性 --> 链接器 --> 常规 --> 附加库目录`添加

	C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1\lib\Win32

- `通用属性 --> CUDA C/C++ --> Common --> CUDA Toolkit Custom Dir`添加

	C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1

- `通用属性 --> 链接器 --> 输入 --> 附加依赖项`添加

	- cuda.lib

	- cudadevrt.lib

	- cudart.lib

	- cudart_static.lib

	- OpenCL.lib

## 10 配置VS+Qt Creator+Android Studio+OpenCV

### 10.0 列出OpenCV的链接库(GitBash)

#### 10.0.1 MSVC链接库文件名

```shell
#Release库文件名
ls D:/Library/OpenCV/OpenCV-MSVC/x64/vc15/lib | grep -P '[^d][\.]lib'

#Debug库文件名
ls D:/Library/OpenCV/OpenCV-MSVC/x64/vc15/lib | grep d.lib
```

#### 10.0.2 MinGW链接库名

去掉lib前缀和.a文件名后缀，使用方式：-lopencv_xxx

```shell
#Release库名
ls D:/Library/OpenCV/OpenCV-MinGW/x64/mingw/lib | grep -P '[^d][\.]a' | cut -d '.' -f 1 | cut -c 4-

#Debug库名
ls D:/Library/OpenCV/OpenCV-MinGW/x64/mingw/lib | grep d.a | cut -d '.' -f 1 | cut -c 4-
```

### 10.1 解压编译好的库压缩包

### 10.2 环境变量`PATH`中添加

```
D:\Library\OpenCV\OpenCV-MSVC\bin
D:\Library\OpenCV\OpenCV-MSVC\x64\vc15\bin
```

### 10.3 VS2017+OpenCV

#### 10.3.1 新建win32应用程序

#### 10.3.2 配置X64库

- `视图 -> 属性管理器`

- 双击`Debug|x64->Microsoft.Cpp.x64.User`

- `通用属性 --> C/C++ --> 常规 --> 附加包含目录`添加

	D:\Library\OpenCV\OpenCV-MSVC\include

- `通用属性 --> 链接器 --> 常规 --> 附加库目录`添加

	D:\Library\OpenCV\OpenCV-MSVC\x64\vc15\lib

- `通用属性 --> 链接器 --> 输入 --> 附加依赖项`添加

	opencv_xxx451.lib

#### 10.3.3 配置X86库(一般不用)

- `视图 -> 属性管理器`

- 双击`Debug|Win32->Microsoft.Cpp.Win32.User`

- `通用属性 --> C/C++ --> 常规 --> 附加包含目录`添加

	D:\Library\OpenCV\OpenCV-MSVC\include

- `通用属性 --> 链接器 --> 常规 --> 附加库目录`添加

	D:\Library\OpenCV\OpenCV-MSVC\x86\vc15\lib

- `通用属性 --> 链接器 --> 输入 --> 附加依赖项`添加

#### 10.3.4 Release/Debug库

Release库只需要在编译时，将`opencv_xxxd.lib`与`opencv_xxx.lib`调换位置

### 10.4 Qt Creator+OpenCV

#### 10.4.1 安装Qt

注意：勾上MSVC2017 64位编译器、CDB调试器

#### 10.4.2 安装CDB

`程序和功能 -> Windows SDK -> 更改 -> Change -> 勾上Debugging Tools`

#### 10.4.3 环境变量`PATH`中添加

注意：因为经常用OpenCV的MSVC库，必须MSVC在前

```
C:\Qt\Qt5.12.10\5.12.10\msvc2017_64\bin
C:\Qt\Qt5.12.10\5.12.10\mingw73_64\bin
```

#### 10.4.4 配置Qt项目文件

注意：LIBS中-L与目录要紧接着，不能留空格

```makefile
INCLUDEPATH += D:/Library/OpenCV/OpenCV-MSVC/include \
				D:/Library/CUDA/samples/common/inc \
				"C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.1/include"
LIBS += -L"C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.1/lib/x64" \
		-lcuda \
		-lcudadevrt \
		-lcudart \
		-lcudart_static \
		-lcudnn
CONFIG(debug, debug|release) {
LIBS += D:/Library/OpenCV/OpenCV-MSVC/x64/vc15/lib/*451d.lib
} else {
LIBS += D:/Library/OpenCV/OpenCV-MSVC/x64/vc15/lib/*451.lib
}
```

### 10.5 Android Studio+OpenCV(纯Java)

#### 10.5.1 配置OpenCV Android-SDK目录

`File -> New -> Import Module` --> `D:\Library\OpenCV\OpenCV-android-sdk\sdk\java`

#### 10.5.2 配置OpenCV Gradle文件

确保导入模块openCVLibrary的build.gradle与项目的app/build.gradle的以下属性一致

- `compileSdkVersion`

- `buildToolsVersion`

- `minSdkVersion`

- `targetSdkVersion`

#### 10.5.3 添加OpenCV Library依赖

`右键项目 -> Open Module Settings -> app -> Dependencies -> + -> Module Dependency -> openCVLibrary`

#### 10.5.4 配置libs

- 把${OpenCV-Android目录}/sdk/native/libs下的所有文件拷贝到项目的app/libs里

- 修改app/build.gradle

```gradle
在dependencies里添加
implementation fileTree(include:'native-libs.jar', dir: "$buildDir/native-libs")
gradle 3.0以下可用
compile fileTree(include:'native-libs.jar', dir: "$buildDir/native-libs")

在dependencies后添加
task nativeLibsToJar(type: Jar, description: 'create a jar archive of the native libs'){
	destinationDir file("$buildDir/native-libs")
	baseName 'native-libs'
	from fileTree(include:'**/*.so', dir: 'libs')
	into 'lib/'
}
tasks.withType(JavaCompile){
	compileTask -> compileTask.dependsOn(nativeLibsToJar)
}
```

#### 10.5.5 点击右上角`Sync Now`

### 10.6 Android Studio+OpenCV(NDK)

#### 10.6.1 重复10.5.1、10.5.2、10.5.3

如果只在NDK中用到OpenCV，可跳过

#### 10.6.2 配置文件(绝对路径)

- 修改app/build.gradle

```gradle
在defaultConfig -> externalNativeBuild -> cmake下添加一行
	abiFilters 'arm64-v8a', 'armeabi-v7a', 'armeabi', 'x86', 'x86_64', 'mips', 'mips64'

在defaultConfig后添加
	sourceSets {
		main {
			jniLibs.srcDirs = ['D:/Library/OpenCV/OpenCV-android-sdk/sdk/native/libs']
		}
	}
```

- 修改app/CMakeLists.txt

```cmake
在cmake_minimum_required(VERSION 3.4.1)后添加
	set(CMAKE_VERBOSE_MAKEFILE on)
	set(ocvlibs "D:/Library/OpenCV/OpenCV-android-sdk/sdk/native/libs")
	include_directories(D:/Library/OpenCV/OpenCV-android-sdk/sdk/native/jni/include)

	add_library(libopencv_java3 SHARED IMPORTED)
	set_target_properties(libopencv_java3 PROPERTIES
						IMPORTED_LOCATION "${ocvlibs}/${ANDROID_ABI}/libopencv_java3.so")

在target_link_libraries -> native-lib行尾添加
	android log libopencv_java3
```

#### 10.6.3 配置文件(相对路径，include与libs的软链接)

- 创建软链接

```bat
mklink /D D:\Projects\Android\OpenCVNDK\app\src\main\cpp\include D:\Library\OpenCV\OpenCV-android-sdk\sdk\native\jni\include
mklink /D D:\Projects\Android\OpenCVNDK\app\src\main\jniLibs D:\Library\OpenCV\OpenCV-android-sdk\sdk\native\libs
```

- 修改app/build.gradle

```gradle
在defaultConfig -> externalNativeBuild -> cmake下添加一行
	abiFilters 'arm64-v8a', 'armeabi-v7a', 'armeabi', 'x86', 'x86_64', 'mips', 'mips64'

在defaultConfig后添加
	sourceSets {
		main {
			jniLibs.srcDirs = ['src/main/jniLibs']
		}
	}
```

- 修改app/CMakeLists.txt

```cmake
在cmake_minimum_required(VERSION 3.4.1)后加上
	set(CMAKE_VERBOSE_MAKEFILE on)
	set(ocvlibs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")
	include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp/include)

	add_library(libopencv_java3 SHARED IMPORTED )
	set_target_properties(libopencv_java3 PROPERTIES
						IMPORTED_LOCATION "${ocvlibs}/${ANDROID_ABI}/libopencv_java3.so")

在target_link_libraries -> native-lib行尾添加
	android log libopencv_java3
```

## 11 安装配置MySQL8

### 11.1 下载地址

- [清华源](https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/)

- [中科大源](https://mirrors.ustc.edu.cn/mysql-ftp/Downloads/)

### 11.2 解压

### 11.3 环境变量`PATH`中添加

```
D:\Library\mysql\bin
```

### 11.4 新建my.ini配置文件

参考D:/Software/Development-Tools/DB/MySQL/config/MySQL8/my.ini

注意：此文件必须是ANSI编码

### 11.5 初始化并创建MySQL服务(管理员CMD/PowerShell)

#### 11.5.1 创建MySQL服务

```bat
cd /D D:\Library\mysql 或 cd D:/Library/mysql

@REM 初始化mysql，生成data文件夹中的文件，控制台输出，记住root初始密码
mysqld --initialize --console

@REM 安装1个MySQL服务，命名mysql（默认服务名为mysql，可命名mysql2）
mysqld --install 或 mysqld --install mysql

@REM 启动服务器
net start mysql
```

#### 11.5.2 修改初始密码

```mysql
mysql -u root -p
ALTER USER root@localhost IDENTIFIED BY '123456';
```

### 11.6 升级(管理员CMD/PowerShell)

#### 11.6.1 删除旧MySQL服务

```bat
net stop mysql
mysqld -remove mysql
rmdir /S /Q D:\Library\mysql
```

#### 11.6.2 重跑11.2、11.4、11.5(全新安装)

### 11.7 一般建库排序规则

| 规则 | 速度 | 精度 |
| ---- | ---- | ---- |
| utf8_general_ci  |  快  |  低 |
| utf8mb4_general_ci  |  快  |  低 |
| utf8_unicode_ci  |  慢  |  高 |
| utf8mb4_unicode_ci  |  慢  |  高 |

### 11.8 MySQL的大小写规则

#### 11.8.1 默认大小写规则

| 名称 | 大小写规则 |
| ---- | ---- |
| 数据库名/表名  |  在Windows下均忽略大小写，在UNIX与Linux下区分大小写 |
| 列名/列别名  |  在所有情况下均忽略大小写 |
| 表别名  |  在所有情况下均区分大小写 |
| 变量名  |  严格区分大小写 |

#### 11.8.2 修改大小写规则

- 修改`my.ini`或`my.conf`

```ini
[mysqld]
lower_case_table_names = 0
```

- 表名区分大小写设置参数`lower_case_table_names`

| lower_case_table_names | 大小写规则 |
| ---- | ---- |
| 0  |  创建的表名称原样保存，查找区分大小写（Linux默认） |
| 1  |  创建的表名称强制以小写保存，查找不区分大小写（Windows默认） |

## 12 访问远程主机MySQL数据库

直接访问远程主机MySQL数据库，需要放行MySQL端口，且需要授权该用户以任意IP登录，不安全，不推荐

SSH访问远程主机MySQL数据库，只需放行SSH端口，不用放行MySQL端口，不用授权该用户以任意IP登录，安全，推荐

### 12.1 授权指定用户

```mysql
GRANT 权限 ON 数据库名.表名 TO 用户名@白名单IP IDENTIFIED BY "数据库账户密码";
GRANT ALL PRIVILEGES ON *.* TO "abc"@"192.168.0.123" IDENTIFIED BY "123456";
GRANT ALL PRIVILEGES ON *.* TO "root"@"%" IDENTIFIED BY "123456";
```

### 12.2 配置连接项

#### 12.2.1 MySQL项

| 配置项 | 值 |
| ---- | ---- |
| 数据库主机IP  |  localhost |
| 数据库用户  |  root |
| 数据库密码  |  123456 |
| 数据库端口  |  3306 |

#### 12.2.2 SSH项

| 配置项 | 值 |
| ---- | ---- |
| SSH主机IP  |  233.233.233.233 |
| 服务器用户  |  root |
| SSH端口  |  22 |
| 登录密码  |  123456 |

### 12.3 放行Linux主机端口

#### 12.3.1 firewall-cmd

```shell
#开放3306端口
firewall-cmd --permanent --zone=public --add-port=3306/tcp

#重载配置，立即生效
firewall-cmd --reload

#查看端口开放情况
firewall-cmd --list-all
```

#### 12.3.2 iptables

```shell
#查看开放端口
iptables -L -n

#临时开放3306端口(重启服务器失效)
iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

#临时关闭3306端口(重启服务器失效)
iptables -D INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

#永久保存端口修改
service iptables save
```

#### 12.3.3 直接修改配置文件

```shell
vim /etc/sysconfig/iptables
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```

## 13 安装配置Node.js

### 13.1 解压

### 13.2 配置环境变量

- 添加环境变量

```
NODE_HOME = D:\Library\node
```

- `PATH`中添加

```
%NODE_HOME%
D:\Library\node_pack\node_global
```

### 13.3 配置

#### 13.3.1 配置文件

`${USERPROFILE}/.npmrc` 或 `%USERPROFILE%/.npmrc`

#### 13.3.2 查看配置

```shell
npm config get
```

### 13.4 配置npm全局安装路径

#### 13.4.1 新建目录

- `D:/Library/node_pack/node_cache`

- `D:/Library/node_pack/node_global`

#### 13.4.2 修改配置

- GitBash

```shell
npm config set cache "D:/Library/node_pack/node_cache"
npm config set prefix "D:/Library/node_pack/node_global"
```

- CMD

```bat
npm config set cache "D:\Library\node_pack\node_cache"
npm config set prefix "D:\Library\node_pack\node_global"
```

### 13.5 配置镜像

#### 13.5.1 配置第三方

```shell
npm config set registry https://registry.npm.taobao.org
npm config set registry https://registry.yarnpkg.com
```

#### 13.5.2 恢复官方

```shell
npm config set registry https://registry.npmjs.org
```

#### 13.5.3 使用cnpm

```shell
npm config set registry https://registry.npmjs.org
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 13.6 安装js包

#### 13.6.1 安装

```shell
npm install -g xxx
cnpm install -g xxx
```

#### 13.6.2 查看

```shell
npm list -g
```

### 13.7 升级Node.js

#### 13.7.1 清除npm缓存

```shell
npm cache clean -f
```

#### 13.7.2 安装n模块

```shell
npm install -g n
```

#### 13.7.3 升级npm

```shell
npm install -g npm
```

#### 13.7.4 升级node

```shell
n ls
n lts
```

### 13.8 卸载Node.js

- 删除环境变量

- 删除安装目录

```shell
rm -rf D:/Library/node D:/Library/node_pack ${USERPROFILE}/.npmrc
```

## 14 安装JS框架

### 14.1 Typescript

#### 14.1.1 安装

```shell
npm install -g typescript
```

#### 14.1.2 编译

```shell
tsc helloworld.ts
```

### 14.2 jQuery

```shell
npm install -g jquery
```

### 14.3 Angular

```shell
npm install -g angular
```

### 14.4 Vue

```shell
npm install -g vue
```