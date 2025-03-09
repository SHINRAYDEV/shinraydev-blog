---
title: 【Flutter】切换按钮开发（canvas画布初上手）
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2023-11-09 11:52:00
updated:
tags: [Flutter,Dart,开发笔记]
categories: 开发
keywords:
description: 
top_img: https://s2.loli.net/2025/03/04/xBPga4rRomqkUnX.webp
comments:
cover: https://s2.loli.net/2025/03/04/xBPga4rRomqkUnX.webp
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

🚀 **需求背景**
在页面设计中，我们需要一个可切换状态的按钮，并且需要在按钮的右下角添加一个带圆角的不规则等腰三角形作为状态指示器。经过调研，发现Flutter现有组件无法直接满足需求，因此决定通过强大的 `Canvas` 自定义绘制来实现。


![image.png](https://www.notion.so/image/attachment%3Acbdd3cd6-7fc5-4bca-8fb7-38c1238d28eb%3Aimage.png?table=block&id=1aa6989f-81d8-8034-896e-dfad1aa06a1c&t=1aa6989f-81d8-8034-896e-dfad1aa06a1c)

**🎨 Canvas 基础概念**

Flutter 提供了强大的 `Canvas` API，能绘制各种自定义图形。使用 `Canvas` 绘图，需要明确两个核心概念：

- **画布（Canvas）**：Flutter中二维坐标系的原点位于左上角，右侧为 X 轴正方向，下方为 Y 轴正方向。
    
    ![image.png](https://www.notion.so/image/attachment%3A258d142e-9789-4188-ac03-b9a573961efb%3Aimage.png?table=block&id=1ab6989f-81d8-8046-b6de-e21def176ca5&t=1ab6989f-81d8-8046-b6de-e21def176ca5)
    
- **画笔（Paint）**：定义绘制的颜色、样式（填充或边框）和宽度等属性。

下面定义一个简单的画笔：

```dart
Paint _paint = Paint()
  ..color = Colors.white
  ..style = PaintingStyle.fill
  ..strokeWidth = 2;
```

**🛠️ 自定义圆角三角形绘制思路**

我们需要绘制的图形由两部分组成：

1. **圆角矩形**：作为按钮底色的背景框。
2. **带圆角的不规则三角形**：作为按钮右下角的状态标识。

实现步骤为：

- 使用 `RRect` 绘制比按钮稍小一圈的圆角矩形。
- 使用 `Path` 定义三角形区域，并用 `clipPath` 裁剪出圆角矩形上的三角形区域。
    
    ![image.png](https://www.notion.so/image/attachment%3A3f632e75-407f-40f8-a14e-6833cc282d2b%3Aimage.png?table=block&id=1ab6989f-81d8-80f9-a671-c4fc9b505e6a&t=1ab6989f-81d8-80f9-a671-c4fc9b505e6a)
    

代码实现示意如下：

```dart
class MyCustomPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    // 定义三角形路径
    var path = Path()
      ..moveTo(size.width, size.height)
      ..lineTo(size.width, size.height - 20)
      ..lineTo(size.width - 20, size.height)
      ..close();

    // 裁剪三角形区域
    canvas.clipPath(path);

    // 绘制圆角矩形
    Rect rect = Rect.fromPoints(const Offset(1, 1), Offset(size.width - 2, size.height - 2));
    RRect rRect = RRect.fromRectAndRadius(rect, const Radius.circular(3.0));
    canvas.drawRRect(rRect, _paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}
```

### 🚧 性能优化

开发过程中，通过日志打印发现按钮动画会导致 Canvas 重复绘制，造成额外的性能消耗。因此，可以通过 `RepaintBoundary` 包裹 Canvas 和按钮，避免不必要的重绘。

优化示例如下：

```dart
RepaintBoundary(
  child: CustomPaint(
    size: const Size(200, 72),
    foregroundPainter: MyCustomPainter(),
    child: RepaintBoundary(
      child: ElevatedButton(
        onPressed: onPressed,
        style: btnStyle ?? ElevatedButton.styleFrom(
          padding: EdgeInsets.zero,
          backgroundColor: Theme.of(context).primaryColor,
        ),
        child: Text(text, style: const TextStyle(color: Colors.white, fontSize: 25)),
      ),
    ),
  ),
);
```

### 📌 封装与可复用性提升

为了提高组件的可复用性，按钮尺寸可以作为参数传入，自动计算和绘制对应的角标。该三角形角标在实际业务场景中常见，例如各类提示角标、状态标识等，美观且实用。

![image.png](https://www.notion.so/image/attachment%3A045f6069-4b7c-43fc-a349-59f136c83de7%3Aimage.png?table=block&id=1ab6989f-81d8-805c-b111-e5751bbf782a&t=1ab6989f-81d8-805c-b111-e5751bbf782a)

**🚩 最终封装组件源码**

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

通过以上方式，成功实现了一个带圆角三角形标识的可切换按钮，并通过合理优化保证了组件性能。

# 📎 参考文章

- [https://api.flutter.dev/flutter/dart-ui/Canvas-class.html](https://api.flutter.dev/flutter/dart-ui/Canvas-class.html)
- [https://book.flutterchina.club/chapter10/custom_paint.html](https://book.flutterchina.club/chapter10/custom_paint.html)
- [https://medium.com/flutteropen/canvas-tutorial-01-how-to-use-the-canvas-in-the-flutter-8aade29ddc9](https://medium.com/flutteropen/canvas-tutorial-01-how-to-use-the-canvas-in-the-flutter-8aade29ddc9)