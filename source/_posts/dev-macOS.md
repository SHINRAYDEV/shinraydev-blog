---
title: 系统开发环境驯服指北 - macOS
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2021-09-26 17:21:04
updated:
tags: [macOS,系统,开发环境]
categories: 技术
keywords:
description:
top_img: https://s2.loli.net/2023/04/25/tE6FP13juA4ObrY.jpg
comments:
cover: https://s2.loli.net/2023/04/25/tE6FP13juA4ObrY.jpg
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---

# 系统开发环境驯服指北 - macOS

还依稀记得初学入门时便听程序员、媒体、大牛、好学生都用Macbook Pro，Windows那能玩游戏是不务正业😜。虽说不是非常在意但穷学生出生的我还是在心中暗暗留下了这一抹好奇，因此在2021年大二下的我还是没忍住犹豫再三走入了macOS的探索之路。

首先放出以上巨魔言论的结论，macOS对于**软件开发**而言确实体验舒适：
* 首先高清渲染及系统的字体、明暗对于长时间办公眼睛观感非常舒适
* 有强大而丰富的开发者社群维护的软件包管理器，不必再配置环境变量，开箱即用
* Unix为底层构建的系统更加符合服务端软件开发思维，稳定而强大，性能强大

因此在此记录一些macOS系统配置技巧，主要为开发方向，使其开发更加方便。

## 系统安装

macOS的相关下载与安装官方说明已足够详细明了，除非是Hackintosh的玩法，后面会单独开文记录，因此在此不过多赘述。

[macOS下载](https://support.apple.com/zh-cn/HT211683)
[macOS可引导安装器制作](https://support.apple.com/zh-cn/HT201372)

## 系统设置

首先最直观常用三大选项菜单栏、访达(Finder)、桌面与程序坞(Dock)

### 菜单栏

### 访达Finder
* 显示当前路径 (「访达」- option+command+p)
  ![隐藏/显示路径](https://s2.loli.net/2023/04/26/eR6acBgQKPJSZ7k.png)
* 侧边栏目录管理（「访达」-「设置」command+,）
![访达设置](https://s2.loli.net/2023/04/26/j1WKQgbi6vLT4YJ.png)
![侧边栏目录管理](https://s2.loli.net/2023/04/26/oAkTaincI81Us3B.png)
* 文件名后缀显示隐藏
![文件后缀开启/隐藏](https://s2.loli.net/2023/04/26/Bs5VTv3ZOrwYGen.png)
* macOS办公利器访达QuickLook空格键一键预览功能拓展
  [QuickLook扩展插件](https://github.com/sindresorhus/quick-look-plugins)
### 程序坞Dock
* 减少程序坞的响应时间
```bash
# 设置启动坞动画时间设置为 0.5 秒
defaults write com.apple.dock autohide-time-modifier -float 0.5 && killall Dock

# 设置启动坞响应时间最短
defaults write com.apple.dock autohide-delay -int 0 && killall Dock
```
```bash
# 恢复启动坞默认动画时间
defaults delete com.apple.dock autohide-time-modifier && killall Dock

# 恢复默认启动坞响应时间
defaults delete com.apple.Dock autohide-delay && killall Dock
```
### 实用指令

* 取消密码安全策略限制
```bash
# 取消4位数密码限制 
pwpolicy -clearaccountpolicies

# 更改密码
passwd
```

* 允许安装任意来源的 App 
```bash
# APP安装开启任何来源
sudo spctl --master-disable
```

* 修改主机名和共享名称
![主机名@共享名称](https://s2.loli.net/2023/04/26/AL2oi9s6lgrVuPN.png)
```bash
# 修改主机名
sudo scutil --set HostName {自定义主机名}
```
```bash
# 修改共享名称
sudo scutil --set ComputerName {自定义电脑名}
```

* 禁止“Are you sure you want to open this application?”提示
```bash
defaults write com.apple.LaunchServices LSQuarantine -bool false
```

* 禁用 crash reporter
```bash
defaults write com.apple.CrashReporter DialogType -string none
```

* 禁止磁盘映像验证
```bash
defaults write com.apple.frameworks.diskimages skip-verify -bool true
defaults write com.apple.frameworks.diskimages skip-verify-locked -bool true
defaults write com.apple.frameworks.diskimages skip-verify-remote -bool true
```

* 修改启动台行和列数
```bash
# 设置列数为 9
defaults write com.apple.dock springboard-columns -int 9

# 设置行数为 6
defaults write com.apple.dock springboard-rows -int 6

# 重启 Dock 生效
killall Dock
```
```bash
# 恢复默认的列数和行数
defaults write com.apple.dock springboard-rows Default
defaults write com.apple.dock springboard-columns Default

# 重启 Dock 生效
killall Dock
```
## 按键整理

* `Enter`：选中一个文件按下`Enter`，直接重命名。
* `Ctrl`：命令行快捷键。
* `Command`：图形界面快捷键。
* `Option`：alt切换功能，切换附属功能，比如点击左上角苹果Logo再按下`option`「关于本机」会切换为「系统信息...」功能。
* `Command + Shift + n`：选中所需归档资料，按下`Command + Shift + n`，会将文件归档在一个文件夹下。
* `Option + Command + v`：剪切，也可以拷贝后，右键+`option`，「粘贴项目」会切换为「项目移到这里」。
* `Command + Option + c`：复制文件路径。
* `Command + Delete`：移到废纸篓。
* `Shift + Command + Delete`：永久删除。
* `Control + Command + Space`：emoji😝
* `Command + Up`：回到上一层文件夹。
* `Command + Down`：文件夹则进入，文件则打开。
* `Ctrl + Left/Right`：切换桌面空间。
* `Ctrl + Command + q`：锁屏。
* `Command + .`：隐藏/显示'.'开头文件。
* `Shift + Command + .`：临时隐藏/显示'.'开头文件。
* `Command + Alt + i`：Chrome/Edge开发者工具，相当于F12。
* `Command + Shift + g`：Finder中如何输入路径直接跳转。
  * `Command + Shift + c`：电脑
  * `Command + Shift + h`：个人
  * `Command + Shift + d`：桌面
  * `Command + Shift + a`：应用程序

## 技巧

## 开发配置
* [安装Xcode](https://apps.apple.com/cn/app/xcode/id497799835?mt=12)
* [安装 Xcode Command Line Tools](https://developer.apple.com/download/all/?q=xcode)
可直接使用命令行安装，但介于一些祖国花朵儿网络不好容易中断失败也可点击上方链接直接跳转下载安装，前提注册准备好一个Apple开发者账号。
```bash
# 安装 xcode 命令行工具
xcode-select --install
```
## 好软推荐
