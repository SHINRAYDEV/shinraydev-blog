---
title: 系统开发环境驯服指北 - Windows
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2021-10-01 16:13:30
updated:
tags: [Windows,系统,开发环境]
categories: 技术
keywords:
description:
top_img: https://s2.loli.net/2023/05/05/DPhCO4VAaTWMBcx.jpg
comments:
cover: https://s2.loli.net/2023/05/05/DPhCO4VAaTWMBcx.jpg
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

## 系统安装
[Windows下载](https://next.itellyou.cn)
[Windows可引导安装器制作](https://rufus.ie/zh/)
### Windows11 TPM2.0 绕过
`shift`+`F10`
```bash
REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassTPMCheck /t REG_DWORD /d 1

REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassSecureBootCheck /t REG_DWORD /d 1
```
### Windows11 联网登陆绕过
`shift`+`F10`
```bash
oobe\bypassnro.cmd 
```