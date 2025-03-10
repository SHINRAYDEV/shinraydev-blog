---
title: 【Flutter】保持底部导航栏中各页面状态
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2024-10-30 17:09:54
updated:
tags:
categories:
keywords:
description:
top_img: https://www.developers-zone.com/wp-content/uploads/2022/08/bottom_navigation_bar-exmp-1-1.gif?t=1ab6989f-81d8-8049-b9fa-d7185f4365f4
comments:
cover: https://www.developers-zone.com/wp-content/uploads/2022/08/bottom_navigation_bar-exmp-1-1.gif?t=1ab6989f-81d8-8049-b9fa-d7185f4365f4
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
## 📌 需求背景

Flutter 默认的 BottomNavigationBar 在页面切换时会销毁不可见的页面并重新创建新的页面，目的是优化性能和减少内存占用。但实际业务中，这种默认行为经常带来不便：页面状态会丢失，切换回来时需要重新初始化，影响用户体验。因此需要探索一种合适的状态持久化方案。

## 🧪 多种方案对比实验

经过调研和实验，整理了以下几种方案：
| 组件 | `AutomaticKeepAliveClientMixin`  | `PageStorage`  | `IndexedStack`  | `Offstage` |
| --- | --- | --- | --- | --- |
| 原理 | 通过 KeepAlive 保持 State | 存储 PageStorageBucket | 同时保持所有页面的 State | 隐藏但保留页面State  |
| 适用场景 | PageView / ListView 内的子项 | 记录滚动位置 | BottomNavigationBar / TabView | 通用跨页面状态保存 |
| 跨页面保存状态 | ❌ 否（仅在 PageView 或 ListView 内生效） | ✅ 是 | ✅ 是 | ✅ 是 |
| 懒加载 | ✅ 是（按需加载） | ❌ 否 | ❌ 否（一次性加载所有页面） | ❌ 否（隐藏时不参与渲染和布局） |

## 📌 方案选择与分析

🔹 AutomaticKeepAliveClientMixin

常用于 ListView 和 PageView 内部子组件的状态保持。但在底部导航等多页面场景下，需要每个页面手动实现，且在 Navigator 切换时容易失效，整体不够方便。

🔹 PageStorage

原理是将状态数据保存到特定的 PageStorageBucket 中，跨页面甚至跨 Navigator 跳转时都可以恢复状态。但缺点也明显：在已有的成熟项目中逐个组件实现过于繁琐，改造成本较高。

🔹 IndexedStack（最终选择）

IndexedStack 简单直接，所有页面都会保存在栈中，不会被销毁，状态天然保留。但问题在于不支持懒加载，启动时所有页面会一次性加载，造成启动变慢。

### 🚧 懒加载优化方案（自研实现）

为了克服 IndexedStack 不支持懒加载的问题，我实现了手动懒加载方案：

实现思路：

初始时只加载当前页面，其余页面用空的 Container() 占位。

当用户点击对应页面时，再动态替换为真实的页面。

实现示例如下：

```dart
List<int> loadedPages = [0]; // 首屏默认加载

var screens = [
  isHomeGuidance ? NoviceIndexRoute() : HomeRoute(key: Key(homeRefreshKey)),
  loadedPages.contains(1) ? AccountRoute(onTabChange: onTabIndexChanged) : Container(),
  loadedPages.contains(2)
      ? (isFIREPlanGuidance ? NoviceFinancialOverviewRoute() : FireCalculatorProfileRoute())
      : Container(),
  loadedPages.contains(3) ? LearnEarn(onTabChange: onTabIndexChanged) : Container(),
];

IndexedStack(index: _currentIndex, children: screens);
```

此方案在用户首次点击底部导航时，才真正加载页面，从而达到懒加载的目的，显著降低启动开销。

## 🎯 进一步优化方案

在进一步探索中，了解到Flutter官方提供的 Offstage 组件，其原理类似于CSS的display:none，页面隐藏时不渲染但仍然保留状态，更省内存且性能更佳。

未来优化中，可以将当前IndexedStack方案替换为Offstage方案，并结合手动懒加载进一步优化性能。

## 📌 总结

IndexedStack 能简单、快速地实现状态持久化，但存在性能问题。

手动懒加载方案可有效缓解启动慢的问题。

Offstage 组件可作为更优秀的替代方案进行进一步优化。

这种方案同样适用于全局滚动监听，比如常见的「双击底部Tab返回顶部」功能的实现，提供更好的用户体验。
