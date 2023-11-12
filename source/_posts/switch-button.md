---
title: 【Flutter】切换按钮开发(canvas画布初上手)
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2023-11-09 11:52:00
updated:
tags: [Flutter,Dart,开发笔记]
categories: 开发
keywords:
description:
top_img: https://s2.loli.net/2023/11/10/Xc5rbMJndOIxhqv.png
comments:
cover: https://s2.loli.net/2023/11/10/Xc5rbMJndOIxhqv.png
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
# Flutter-做一个SwitchButton，Canvas画布初上手

## 需求

页面设计需要一个可以切换和不切换的按钮，右下角有一个小三角形作为能否切换的标识，增强按钮的交互性和可识别性。由于指示小三角为等腰三角且带有圆角的不规则图形(如下图所示)，搜索没有发现存现成组件，所以思考如何制作一个。
![SwitchButton](https://s2.loli.net/2023/11/10/Xc5rbMJndOIxhqv.png)

## 实现方式

经请教Mentor得知Canvas自定义画布很强大，可以实现自定义的任意图形，所以先去学习了下基础的Flutter Canvas绘画。首先最简单的的Canvas画布逻辑正如其名，我们需要一个画布和画笔。

```dart
/// 先定义一下画笔的类型
Paint _paint = Paint()
  ..color = Colors.white /// 白色
  ..style = PaintingStyle.fill /// 填充
  ..strokeWidth = 2; /// 画笔宽度

/// 创建一个自定义画布用画笔开始绘画
class MyCustomPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    var path = Path()
      ..moveTo(50, 36)
      ..lineTo(50, 26)
      ..lineTo(40, 36)
      ..close();
    canvas.clipPath(path);
    Rect rect = Rect.fromPoints(const Offset(1, 1), const Offset(49, 35));
    RRect rRect = RRect.fromRectAndRadius(rect, const Radius.circular(3.0));
    canvas.drawRRect(rRect, _paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}
```

首先画布是2D的，那么便可以用二维坐标系来定位画笔进行绘画任意图形。Flutter官方规定画布的二维坐标系原点位于***左上角***，右横向为X轴正方向，纵下方向为Y轴正方向。
![canvas](https://s2.loli.net/2023/11/10/23VSpw6DyjlnU8u.png)
圆角三角形指示器的实现思路为，直接使用 dart ui 的 RRect class ，直接绘制出圆角矩形，然后裁出所需要的三角，实现思路图示如下
![实现思路](https://s2.loli.net/2023/11/11/UMmBOfgR1TdivSj.png)
## 优化
完成后发现会出现canvas画布会因按钮动画造成多次重绘消耗性能，因此为组件包裹上RepaintBoundary，阻隔按钮动画与canvas以减少重绘进行优化。
还可以做的优化便是分离长宽大小计算。
最后放上源码
```dart
/// 切换按钮集成
class _SwitchButton extends StatelessWidget {
  const _SwitchButton({required this.text, this.switchable = true, this.btnStyle, this.onPressed});

  final String text;

  /// 预留参数-按钮的可切换/不可切换，切换提示小三角形显示与否
  final bool switchable;
  final ButtonStyle? btnStyle;
  final VoidCallback? onPressed;

  @override
  Widget build(BuildContext context) {
    return SizedBox(
        width: 200,
        height: 72,
        child: switchable
            ? RepaintBoundary(
                child: CustomPaint(
                    size: const Size(200, 72),
                    foregroundPainter: MyCustomPainter(),

                    /// 优化：阻隔按钮动画与canvas防止重绘
                    child: RepaintBoundary(
                        child: ElevatedButton(
                      onPressed: onPressed,
                      style: btnStyle ??
                          ElevatedButton.styleFrom(
                              padding: const EdgeInsets.all(0), backgroundColor: Theme.of(context).primaryColor),
                      child: Text(text, style: const TextStyle(color: Colors.white, fontSize: 25)),
                    ))))
            : ElevatedButton(
                onPressed: onPressed,
                style: btnStyle ??
                    ElevatedButton.styleFrom(
                        padding: const EdgeInsets.all(0), backgroundColor: Theme.of(context).primaryColor),
                child: Text(text, style: const TextStyle(color: Colors.white, fontSize: 15))));
  }
}

Paint _paint = Paint()
  ..color = Colors.white
  ..style = PaintingStyle.fill
  ..strokeWidth = 2;

/// List按钮小三角绘制
class MyCustomPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    var path = Path()
      ..moveTo(200, 72)
      ..lineTo(200, 52)
      ..lineTo(180, 72)
      ..close();
    canvas.clipPath(path);
    Rect rect = Rect.fromPoints(const Offset(1, 1), const Offset(198, 70));
    RRect rRect = RRect.fromRectAndRadius(rect, const Radius.circular(3.0));
    canvas.drawRRect(rRect, _paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}
```