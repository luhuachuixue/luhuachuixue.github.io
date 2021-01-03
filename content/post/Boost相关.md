---
title: "Boost相关"
date: "2021-01-03T19:26:22+08:00"
slug: 576e87c1
tags: [Boost]
categories: [开发]
draft: false
---

## 0 简介

C++标准委员会成员发起成立Boost社区，提供C++标准候选特性，被称为“准”标准库

- [官网](https://www.boost.org)

- [下载地址一](https://dl.bintray.com/boostorg/release/)

- [下载地址二](https://sourceforge.net/projects/boost/files/)

## 1 脚本编译

[编译脚本下载地址](https://github.com/luhuachuixue/build-script)

### 1.1 MSVC(15分钟)

- 修改脚本build.cmd，放在源码压缩包同级目录

| 变量 | 说明 |
| ---- | ---- |
| boost_version  |  对应Boost版本 |
| boost_install_dir  |  对应Boost库安装目录 |
| tool_set  |  MSVC版本 |
| msvc_env  |  对应MSVC环境配置BAT脚本路径 |

- 双击脚本build.cmd

- GitBash重新组织include目录打包

```shell
cd D:/Library/libboost/include && mv boost-1_75/boost . && rm -rf boost-1_75
sed -i 's:include/boost-1_75:include:g' `grep -rl 'include/boost-1_75' D:/Library/libboost/lib/cmake`
rm D:/Software/Development-Tools/3rd-libs/STL/Boost/libboost-msvc.7z
cd D:/Library && 7z a -t7z -mmt8 D:/Software/Development-Tools/3rd-libs/STL/Boost/libboost-msvc.7z libboost
```

### 1.2 MinGW(15分钟)

- 修改脚本build.cmd，放在源码压缩包同级目录

| 变量 | 说明 |
| ---- | ---- |
| boost_version  |  对应Boost版本 |
| boost_install_dir  |  对应Boost库安装目录 |

- 双击脚本build.cmd

- GitBash重命名打包

```shell
renamex -Rs :-mt-x64::g D:/Library/libboost/lib
renamex -Rs :-mt-d-x64:-d:g D:/Library/libboost/lib
sed -i 's:-mt-x64::g' `grep -rl '\-mt-x64' D:/Library/libboost/lib/cmake`
sed -i 's:-mt-d-x64:-d:g' `grep -rl '\-mt-d-x64' D:/Library/libboost/lib/cmake`
rm D:/Software/Development-Tools/3rd-libs/STL/Boost/libboost-mingw.7z
cd D:/Library && 7z a -t7z -mmt8 D:/Software/Development-Tools/3rd-libs/STL/Boost/libboost-mingw.7z libboost
```

- 环境变量`PATH`中添加

```
D:\Library\libboost\bin
```

## 2 列出链接库(GitBash)

### 2.1 MSVC链接库文件名

```shell
#Release库文件名
ls D:/Library/libboost/lib | grep -P 'mt-x64-[\d]+_[\d]+.lib'

#Debug库文件名
ls D:/Library/libboost/lib | grep -P 'mt-gd-x64-[\d]+_[\d]+.lib'
```

### 2.2 MinGW链接库名

去掉lib前缀和.a文件名后缀，使用方式：-lboost_xxx

```shell
#Release库名
ls D:/Library/libboost/lib | grep -P '[^d][\.]a' | cut -d '.' -f 1 | cut -c 4-

#Debug库名
ls D:/Library/libboost/lib | grep d.a | cut -d '.' -f 1 | cut -c 4-
```

### 2.3 库文件命名格式

| 字符 | 说明 |
| ---- | ---- |
| lib前缀  |  MSVC的静态库 或 GCC的动/静态库 |
| mt  |  多线程 |
| s  |  静态链接C++标准库和编译器运行时 |
| g  |  链接debug版本的C++标准库和编译器运行时，也即msvcrtd.lib、libcmtd.lib |
| d  |  debug |

- 格式

   ` (lib)boost_xxx(库名称)_xxx(编译器版本)-xxx(多线程方式)-xxx(OS位数)-x.x.x(Boost版本).lib|dll|a|so`

- 举例

    `boost_regex-vc141-mt-x64-1_75.lib`
    `libboost_regex-mgw10-mt-x64-1_75.a`

## 3 编译参数

### 3.1 查看需要编译的库

注意：有些库，不需要编译，而是以头文件方式引用

```shell
./b2 --show-libraries
```

### 3.2 编译参数

```shell
./b2 --help
```

| 参数 | 说明 |
| ---- | ---- |
| --prefix=  |  install下，头、库文件根目录，如：D:/Library/libboost |
| --stagedir=  |  stage下，库文件根目录，默认：stage/lib |
| --layout=  |  tagged: 库文件不带编译器名称版本号，Boost版本号，但带多线程和平台标识，头文件直接放在boost目录 <br> versioned: Windows平台默认，库文件带所有版本号和标识，头文件放在Boost版本号子目录 <br> system: Linux平台默认，库文件不带任何版本和标识，头文件直接放在boost目录 |
| --build-type=  |  minimal: 尽可能少编译库 <br> complete: 同时生成Debug和Release版本库 |
| --without-xxx  |  不编译xxx库，如：--without-python表示不编译Python库 |
| install  |  同时生成头、库文件 |
| stage  |  只生成库文件 |
| variant=  |  可选：debug、release |
| threading=  |  单/多线程，可选：single、multi |
| link=  |  动/静态库，可选：shared、static |
| runtime-link=  |  动/静态链接C/C++运行时，可选：shared、static |
| target-os=  |  目标平台操作系统，如：windows、linux |
| architecture=  |  目标平台CPU架构，如：x86 |
| address-model=  |  目标平台指令集寻址位数，如：64、32 |

## 4 头文件宏定义指定库链接方式

参考`boost/config/user.hpp`

Boost使用一种所谓用户配置的方式，头文件中使用宏来决定使用动/静态库

不想使用这种用户配置方式，可以参考`4.3`，可以使用任意名称的动/静态库

### 4.1 静态库

默认没有定义相应的宏，则使用静态库

库名字格式固定为`libboost_xxx-vc141-mt-s-x64-1_75.lib`

需要使用默认的`--layout`设置编译Boost静态库（也即b2编译时，不带参数`--layout`），确保库名格式一致

### 4.2 动态库

定义`BOOST_ALL_DYN_LINK`、`BOOST_XXX_DYN_LINK`，用来生成`__declspec(dllimport)`

#### 4.2.1 方式一、编译器带参数宏

`通用属性 --> C/C++ --> 预处理器 --> 预处理器定义`添加`BOOST_ALL_DYN_LINK`、`BOOST_XXX_DYN_LINK`

#### 4.2.2 方式二、引用Boost头文件前加宏定义

```c++
// 所有库使用动态链接库
#define BOOST_ALL_DYN_LINK

// XXX库使用动态链接库
#define BOOST_XXX_DYN_LINK
```

### 4.3 常规链接格式

定义`BOOST_NO_CONFIG`宏，取消Boost的默认的库链接配置方式，再按照常规方式指定任意链接库名

#### 4.3.1 方式一、编译器带参数宏

`通用属性 --> C/C++ --> 预处理器 --> 预处理器定义`添加`BOOST_NO_CONFIG`

#### 4.3.2 方式二、引用Boost头文件前加宏定义

```c++
#define BOOST_NO_CONFIG
```