---
title: 【Flutter】Flutter/Dart 学习 & Flutter 开发踩坑备忘录
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2023-11-11 00:27:56
updated:
tags: [Flutter,Dart,学习,踩坑]
categories: 技术
keywords:
description:
top_img: https://s2.loli.net/2023/11/12/JF5inSWTQ1rR3Ea.webp
comments:
cover: https://s2.loli.net/2023/11/12/JF5inSWTQ1rR3Ea.webp
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

## Flutter/Dart 学习
### Flutter 测试覆盖率
测试覆盖率生成/更新
```bash
flutter test --coverage
```
测试旅覆盖网页可视化生成
```bash
# 安装生成器依赖
brew install lcov
# 生成可视网页
genhtml coverage/lcov.info -o coverage/html
# 打开生成的网页
open coverage/html/index.html
```
### Golden File Test
生成和更新基线
静态资源如图片和字体需写入测试提前载入
```bash
flutter test --update-goldens
# 指定文件
flutter test --update-goldens <path_to_test_file>
```

## Flutter 开发踩坑备忘录
### flutter pub run build_runner build failed
```bash
flutter pub get && flutter pub run build_runner build --delete-conflicting-outputs
```

### File not found: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/arc/libarclite_iphoneos.a
更改 Podfile 的 installer 部分如下
```pod
post_install do |installer|
  installer.generated_projects.each do |project|
    project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.0'
         end
    end
end
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
  end
end
```