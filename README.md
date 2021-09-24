# 魔兽世界大脚插件更新工具 <!-- omit in toc -->

一个用于自动更新魔兽世界大脚插件的工具，属于下载器一类。

## 目录 <!-- omit in toc -->

- [构建二进制文件](#构建二进制文件)
  - [方法 1 容器编译](#方法-1-容器编译)
  - [方法 2 在线编译](#方法-2-在线编译)
  - [方法 3 本地编译](#方法-3-本地编译)
- [使用方法](#使用方法)
- [小提示](#小提示)
- [原理](#原理)
  - [跨大版本更新方案](#跨大版本更新方案)
- [须知](#须知)
- [参考](#参考)
- [许可](#许可)

## 构建二进制文件

- 以下所有操作工作目录均为当前项目

### 方法 1 容器编译

- 容器编译需要你自备 spec 文件，使用[方法 3](#方法-3)可以生成 spec。
- 值得注意的是 /wine/drive_c/src 为容器默认工作目录链接到 /src
- 所以要把源码所在目录挂载到容器中的 /src 目录可以避免一些低级错误

```bash
# 构建完毕后生成 ./dist/windows/main.exe
docker run -v "$(pwd)/src:/src" cdrx/pyinstaller-windows:python3 # 发布用
docker run -v "$(pwd)/src:/src" cdrx/pyinstaller-windows:python3 "pyinstaller -D main.py"  # 测试用
```

### 方法 2 在线编译

1. Fork 本项目，进入 Actions 手动运行工作流等待构建完毕，下载构建好的工件。
2. 打开本项目的 [Actions](https://github.com/Sdator/bigfootUpData/actions) 找到顶部最新的并构建成功的任务(带有绿色打勾的图标)点击进去，直接下载里面构建好的工件，当然这个有期限的，如果没过期了请自行构建。

### 方法 3 本地编译

- Windows10 本地环境编译，确保安装了 python 3.x
- 依赖
  - pip install aiohttp
  - pip install pyinstaller

```bash
pip install aiohttp pyinstaller         # 安装依赖
cd src                                  # 进入源代码目录
pyinstaller -F -w main.py -i wow.ico    # 生成二进制  .dist/main.exe
```

## 使用方法

运行程序，弹出的文件对话框中选择你的魔兽世界游戏更目录确认即可，等待数秒（根据网速而定），程序在后台自动下载最新版的大脚并且会放到本目录中（安装完成可删除），之后会自动安装，安装完成后有信息框提示。

## 小提示

如果你不小心选错了目录，程序无法判断游戏目录的正确性，所以它仍然会在你选择的目录下安装插件，你可以手动删除，或马上关闭程序，此时程序生成的配置文件中记录的还是你原来选择的错误路径，你可以用文本方式打开配置文件（切勿用自带的记事本打开）把键"游戏路径"的值改为正确的游戏路径。

推荐：另外一个方法就是 删除配置文件即（配置.json）可以解决大部分问题。（如果看不懂下面的采用此方法即可）

最后：如没必要请切勿随意修改配置文件可能会造成程序发生未知错误，如果你非要打开（切勿用自带的记事本打开修改保存）（切勿用自带的记事本打开修改保存）（切勿用自带的记事本打开修改保存），请用 vscode、notepad++等能随时查看文件编码的编辑器，编码保存为无 bom 的 utf8。

## 原理

本程序采用了版本号递增法，无法判断大版本的更新, 如果要解决这个问题是非常浪费网络资源，所以我默认了 10 个计数器可以从当前版本起检测未来的十个版本，也就是说如果你很久没更新了而且超过了 10 个版本的迭代时间那么可以手动调整线程的值，当然这是不推荐的，因为它很耗费网络资源可能会堵塞网络。

### 跨大版本更新方案

推荐的解决的方案是修改配置中"当前版本"的值为最新版本。 如:"当前版本": "1.xx.x.18" , 你从网络中得知最新的版本为"2.x.x.100", 此时你只需要把 "当前版本": "1.xx.x.18" 改成"当前版本": "2.x.x.99"， 一般来说越靠前的版本号越少变化，所以当出现大版本更新时可手动修改 x/xx 的地方。

## 须知

本工具起初主要为了方便自身而开发，应群友要求补充了一下说明公开给大家使用。

本程序时基于 python 开发，使用 pyinstaller 打包，如发生任何未知因素造成您的计算器损坏等问题责任自负，在确认安全问题之前可使用虚拟机运行。

**本程序是属于下载器一类，只负责下载更新文件，不保证大脚插件本身的安全性，如果是大脚插件出了问题请到大脚官方论坛反映，谢谢。**

> 作者：絕版大叔、 QQ:250740270
> 基友 q 群：流放拾荒者 253529549
> 2019.10.6 AM

## 参考

- [docker-pyinstaller](https://github.com/cdrx/docker-pyinstaller): 编译二进制
- [pyinstaller-action-windows](https://github.com/JackMcKew/pyinstaller-action-windows)：工作流配置相关
- [pyinstaller-action-windows-example](https://github.com/JackMcKew/pyinstaller-action-windows-example): 工作流模板

## 许可

本项目中的脚本和文档是在 [MIT 许可](https://github.com/actions/upload-artifact/blob/main/LICENSE)下发布的
