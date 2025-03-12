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
### 「.」和「..」
调用「.」返回的则是该方法返回的值 
调用「..」后返回的相当于是 this
### 生命周期
#### StatelessWidget 无状态组件
无状态组件不可变，它的属性不能变化，所有值都是最终的，其生命周期只有build这个过程。
其构造方法通常只会在三种情况下被调用：
* 小组件第一次被插入树中
* 小组件的父组件改变其配置
* 它所依赖的 InheritedWidget 发生变化时
#### StatefulWidget 有状态组件
* **createState** 
该函数为 StatefulWidget 中创建 State 的方法，当 StatefulWidget 被创建时会立即执行 createState。createState 函数执行完毕后表示当前组件已经在 Widget 树中，此时有一个非常重要的属性 `mounted` 被置为 true
* **initState** 
该函数为 State 初始化调用，只会被调用一次，因此，通常会在该回调中做一些一次性的操作，如执行 State 各变量的初始赋值、订阅子树的事件通知、与服务端交互，获取服务端数据后调用 setState 来设置 State
* **didChangeDependencies** 
该函数是在该组件依赖的 State 发生变化时会被调用。这里说的 State 为全局 State，例如系统语言 Locale 或者应用主题等，Flutter 框架会通知 widget 调用此回调。该方法调用后，组件的状态变为 `dirty`，立即调用 build 方法
* **build** 
主要是返回需要渲染的 Widget，由于 build 会被调用多次，因此在该函数中只能做返回 Widget 相关逻辑，避免因为执行多次而导致状态异常
* **reassemble** 
主要在开发阶段使用，在 debug 模式下，每次热重载都会调用该函数，因此在 debug 阶段可以在此期间增加一些 debug 代码，来检查代码问题。此回调在 release 模式下永远不会被调用
* **didUpdateWidget** 
该函数主要是在组件重新构建，比如说热重载，父组件发生 build 的情况下，子组件该方法才会被调用，其次该方法调用之后一定会再调用本组件中的 build 方法
* **deactivate** 
在组件被移除节点后会被调用，如果该组件被移除节点，然后未被插入到其他节点时，则会继续调用 dispose 永久移除
* **dispose** 
永久移除组件，并释放组件资源。调用完 dispose 后，`mounted` 属性被设置为 false，也代表组件生命周期的结束
![flutter-lifecycle](https://s2.loli.net/2023/11/27/iIxEgFcoLSl2z1u.png)
### Flutter 弹窗状态更新
Flutter StatefulWidget 常使用 setState 方法更新与当前 Widget 相关的状态，其只能重新构建当前 Widget 及其子 Widget 。而弹窗一般由 showDialog 方法创建，返回一个 Future 对象，表示弹窗的结果。所以弹窗是一个独立的组件，它位于屏幕的顶层，与主 Widget 的状态并不直接相关，因此主组件的 setState 方法对其不构成影响。要实现更新弹窗中的状态，可以使用 StatefulBuilder 或者自定义的 StatefulWidget 来实现
如若功能较为简单可以使用 StatefulBuilder，而较为复杂的操作联动弹窗建议提出为 StatefulWidget 组件，以下为 StatefulBuilder 使用方式
```dart
await showDialog<void>(
  context: context,
  builder: (BuildContext context) {
    int? selectedRadio = 0;
    return AlertDialog(
      content: StatefulBuilder(
        builder: (BuildContext context, StateSetter setState) {
          return Column(
            mainAxisSize: MainAxisSize.min,
            children: List<Widget>.generate(4, (int index) {
              return Radio<int>(
                value: index,
                groupValue: selectedRadio,
                onChanged: (int? value) {
                  setState(() => selectedRadio = value);
                },
              );
            }),
          );
        },
      ),
    );
  },
);
```
### Flutter 与原生通信
Flutter 通过 `Platform Channels` 与原生通信，`Platform Channels` 是一种消息传递机制，允许Flutter代码与原生平台代码进行**双向通信**，主要有三种类型的通道：
* MethodChannel：用于传递方法调用
* EventChannel：用于数据流的通信，例如持续的传感器数据
* BasicMessageChannel：用于传递字符串和半结构化的消息
以下为 MethodChannel 的使用方式
Flutter中
```dart
// 创建一个MethodChannel对象
const platform = MethodChannel('com.example.myapp/channel');

// 调用原生方法
Future<void> callNativeFunction() async {
  try {
    final result = await platform.invokeMethod('myNativeFunction');
    // 使用result
  } on PlatformException catch (e) {
    // 处理可能出现的异常
  }
}
```
Android 中
```kotlin
class MainActivity: FlutterActivity() {
  override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
    super.configureFlutterEngine(flutterEngine)
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, "com.example.myapp/channel")
      .setMethodCallHandler { call, result ->
        if (call.method == "myNativeFunction") {
          // 在这里执行原生的方法
          val nativeResult = "Hello from native"
          result.success(nativeResult)
        } else {
          result.notImplemented()
        }
      }
  }
}
```
iOS 中
```swift
import Flutter

class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    
    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController
    let channel = FlutterMethodChannel.init(name: "com.example.myapp/channel", binaryMessenger: controller.binaryMessenger)
    
    channel.setMethodCallHandler({
      (call: FlutterMethodCall, result: FlutterResult) -> Void in
      if (call.method == "myNativeFunction") {
        // 在这里执行原生的方法
        let nativeResult = "Hello from native"
        result(nativeResult)
      } else {
        result(FlutterMethodNotImplemented)
      }
    })

    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```
### Flutter 多线程
Flutter的Isolate提供了一种并发执行代码的方式，可以在多个Isolate之间并行执行任务，从而提高应用程序的性能和响应能力。每个Isolate都是相互独立的，拥有自己的事件循环、堆内存和栈，可以执行独立的计算任务、IO操作等。以下是 Isolate 的使用方式
Isolate
```dart
import 'dart:isolate';

// 一个耗时的计算函数，它将在新的Isolate中运行
void expensiveComputation(Map<String, dynamic> message) {
  final sendPort = message['sendPort'] as SendPort;
  final int data = message['data'] as int;

  int sum = 0;
  for (int i = 0; i <= data; i++) {
    sum += i;
  }

  // 将结果发送回主Isolate
  sendPort.send(sum);
}
```
```dart
void main() async {
  final receivePort1 = ReceivePort();
  final receivePort2 = ReceivePort();

  await Isolate.spawn(expensiveComputation, {
    'sendPort': receivePort1.sendPort,
    'data': 100000000,
  });

  await Isolate.spawn(expensiveComputation, {
    'sendPort': receivePort2.sendPort,
    'data': 200000000,
  });

  final result1 = await receivePort1.first;
  final result2 = await receivePort2.first;

  print('Result from first isolate: $result1');
  print('Result from second isolate: $result2');
}
```
需要注意的是，由于Isolate是相互独立的，因此不能直接访问主线程的UI组件和状态。如果需要更新UI或与UI交互，可以通过消息传递将结果返回给主线程，然后由主线程来更新UI。
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