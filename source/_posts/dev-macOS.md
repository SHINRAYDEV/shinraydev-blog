---
title: 【macOS】Sonoma 环境配置备忘
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2021-09-26 17:21:04
updated:
tags: [macOS,系统,开发环境]
categories: 技术
keywords:
description:
top_img: https://s2.loli.net/2025/03/12/6kcJynZHYip8Nsm.webp
comments:
cover: https://s2.loli.net/2025/03/12/6kcJynZHYip8Nsm.webp
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

# Sonoma环境配置备忘 - macOS

## 系统安装

macOS的相关下载与安装官方说明已足够详细明了，除非是Hackintosh的玩法，后面会单独开文记录，在此不再赘述。

[macOS 下载](https://support.apple.com/zh-cn/HT211683)
[macOS 可引导安装器制作](https://support.apple.com/zh-cn/HT201372)

## 系统设置

### 常用指令

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

* Dock(程序坞)设置
```bash
# 自动隐藏 Dock
defaults write com.apple.dock autohide -bool true

# 取消 Dock 默认显示的“最近打开应用”
defaults write com.apple.dock show-recents -bool false

# 调整 Dock 图标大小
defaults write com.apple.dock tilesize -int 58
```
  * 减少Dock(程序坞)响应时间
  ```bash
# 设置启动坞动画时间设置为 0.5 秒
defaults write com.apple.dock autohide-time-modifier -float 0.5 && killall Dock

# 设置启动坞响应时间最短
defaults write com.apple.dock autohide-delay -int 0 && killall Dock
  ```
  恢复默认设置
  ```bash
# 恢复启动坞默认动画时间
defaults delete com.apple.dock autohide-time-modifier && killall Dock

# 恢复默认启动坞响应时间
defaults delete com.apple.Dock autohide-delay && killall Dock
  ```

* 修改Launchpad(启动台)行和列数
```bash
# 设置列数为 9
defaults write com.apple.dock springboard-columns -int 9

# 设置行数为 6
defaults write com.apple.dock springboard-rows -int 6

# 重启 Dock 生效
killall Dock
```
恢复默认设置
```bash
# 恢复默认的列数和行数
defaults write com.apple.dock springboard-rows Default
defaults write com.apple.dock springboard-columns Default

# 重启 Dock 生效
killall Dock
```

* 修改主机名和共享名称
```bash
# 修改主机名
sudo scutil --set HostName {自定义主机名}

# 修改共享名称
sudo scutil --set ComputerName {自定义电脑名}
```

* 禁用输入时的自动 quotes、dashes 转换

```bash
defaults write -g NSAutomaticQuoteSubstitutionEnabled -bool false
defaults write -g NSAutomaticDashSubstitutionEnabled -bool false
```

* 禁止在 double space 时添加句点
  
```bash
defaults write -g NSAutomaticPeriodSubstitutionEnabled -bool false
```

* 禁止输入时自动将 word 首字母大写
  
```bash
defaults write -g NSAutomaticCapitalizationEnabled -bool false
```

* 默认搜索范围，改为当前目录
  
```bash
defaults write com.apple.finder FXDefaultSearchScope -string SCcf
```

## 开发配置

* [安装Xcode](https://apps.apple.com/cn/app/xcode/id497799835?mt=12)
  *如果不想登陆 App Store 下载安装也可点击下方链接下载*
* [安装 Xcode Command Line Tools](https://developer.apple.com/download/all/?q=xcode)
  *可直接使用命令行安装，但如果网络环境不好容易中断失败也可点击上方链接直接跳转下载安装，需提前注册准备好一个Apple开发者账号*
```bash
# 安装 xcode 命令行工具
xcode-select --install
```

* 显示当前路径 (「访达」- <kbd>option</kbd>+<kbd>command</kbd>+<kbd>p</kbd>)
  
```bash
defaults write com.apple.finder ShowPathbar -bool true
```

* 文件名后缀显示 / 隐藏
  
```bash
defaults write -g AppleShowAllExtensions -bool true
```

* 禁用修改扩展名时的警告
  
```bash
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false
```

* [🍺Homebrew](https://brew.sh)
强大便捷高效的包管理器，可以避免沾到中文互联网畸形大粪池，其能够管理包下所有视图软件和非视图软件的登陆项、后台运行管理和版本管理


* [QuickLook拓展插件](https://github.com/sindresorhus/quick-look-plugins)

```bash
brew install qlcolorcode qlstephen qlmarkdown quicklook-json qlimagesize suspicious-package apparency quicklookase qlvideo
```

* 允许选择 QuickLook 中的文本
*能看不让选，功能少一半*

```bash
defaults write com.apple.finder QLEnableTextSelection -bool true
```

## 好软推荐

| 软件 | 软件 | 软件 | 软件 |
|:---:|:---:|:---:|:---:|
| <img src="https://s2.loli.net/2024/01/01/iTerm2.png" width="64"><br>iTerm2 | <img src="https://s2.loli.net/2024/01/01/VSCode.png" width="64"><br>Visual Studio Code | <img src="https://s2.loli.net/2024/01/01/Alfred.png" width="64"><br>Alfred | <img src="https://s2.loli.net/2024/01/01/Rectangle.png" width="64"><br>Rectangle |
| <img src="https://s2.loli.net/2024/01/01/Raycast.png" width="64"><br>Raycast | <img src="https://s2.loli.net/2024/01/01/CleanMyMac.png" width="64"><br>CleanMyMac X | <img src="https://s2.loli.net/2024/01/01/Bartender.png" width="64"><br>Bartender | <img src="https://s2.loli.net/2024/01/01/AltTab.png" width="64"><br>AltTab |
| <img src="https://s2.loli.net/2024/01/01/Keka.png" width="64"><br>Keka | <img src="https://s2.loli.net/2024/01/01/Typora.png" width="64"><br>Typora | <img src="https://s2.loli.net/2024/01/01/Docker.png" width="64"><br>Docker | <img src="https://s2.loli.net/2024/01/01/Postman.png" width="64"><br>Postman |
