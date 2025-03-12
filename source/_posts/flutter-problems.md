---
title: 【Flutter】踩坑日志
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2025-03-12 15:24:17
updated:
tags:
categories:
keywords:
description:
top_img:
comments:
cover:
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
### Flutter 3.29.1 开始 iOS 编译 Xcode 报错 DerivedData 相关问题

Xcode ⇒ File ⇒ Workspace Settings

Derived Data 修改为 CustomLocation，目录尾部补加2创建新目录

![image.png](https://www.notion.so/image/attachment%3Ac7b2e5d1-cad9-4402-965e-03128c80b962%3Aimage.png?table=block&id=1b46989f-81d8-804f-9311-c85828790c09&t=1b46989f-81d8-804f-9311-c85828790c09)

### Android Studio Ladybug 升级后 Flutter Android 构建开始报错 JDK 编译错误

因为新版本AS自带捆绑的JDK版本超过了Flutter项目使用的JDK版本，需要强制转回项目所用JDK版本

```bash
flutter config --jdk-dir [JDK 17 路径]
```

### Flutter 开屏页后黑屏一阵才进入页面

Info.plist文件中查找键值对

```jsx
<key>UILaunchStoryboardName</key>
<string>LaunchScreen.storyboard</string>
```

改为

```jsx
<key>UILaunchStoryboardName</key>
<string>LaunchScreen</string>
```

### Flutter iOS 下状态栏消失/颜色管理不正常

大概率因为业务页面没有使用AppBar，Flutter的AppBar会自动应用`SystemUiOverlayStyle`

根据AppBar的backgroundColor决定状态栏的颜色和状态栏图标颜色，如果页面不存在AppBar或者AppBar颜色为透明色则需要手动声明管理颜色

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class MyDynamicPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
  // 包入颜色管理声明
    return AnnotatedRegion<SystemUiOverlayStyle>(
      value: SystemUiOverlayStyle(
        statusBarColor: Colors.green, // 绿色状态栏
        statusBarIconBrightness: Brightness.light, // 白色图标
      ),
      child: Scaffold(
        appBar: AppBar(title: Text("动态状态栏")),
        body: Center(child: Text("状态栏颜色已改变")),
      ),
    );
  }
}
```

以上为单页面 Scaffold 状态栏颜色管理，全局方式为以下方式

```dart
  // 管理是否显示
  SystemChrome.setEnabledSystemUIMode(SystemUiMode.manual,
      overlays: [SystemUiOverlay.top, SystemUiOverlay.bottom]);
  // 管理显示样式
  SystemChrome.setSystemUIOverlayStyle(const SystemUiOverlayStyle(
    statusBarColor: Colors.transparent,
    systemNavigationBarColor: Colors.white,
    systemNavigationBarIconBrightness: Brightness.dark,
    statusBarIconBrightness: Brightness.dark,
    statusBarBrightness: Brightness.light,
  ));
```

### AppBar 在滚动时变色/颜色加深

Flutter3.7.0时引入的Material风格AppBar属性，用于控制 `AppBar` 在滚动到内容下方时的阴影高度，去除方式如下：

```dart
ThemeData(appBarTheme: const AppBarTheme(scrolledUnderElevation: 0.0))
```

### **flutter pub run build_runner build failed**

```bash
flutter pub get && flutter pub run build_runner build --delete-conflicting-outputs
```

### **File not found: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/arc/libarclite_iphoneos.a**

更改 Podfile 的 installer 部分如下

```bash
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
