---
title: 在 SwiftUI 中如何对 View 进行缩放：scale
layout: post
date: 2020-08-06
---

SwiftUI 提供的 **scaleEffect** 修饰符可以对 View 进行缩放。 

```swift
/// 放大2倍 
Text("swiftxiaozhuanlan.com")
	.scaleEffect(2)
/// 单独对 x、y 缩放不同的倍数
Text("swiftxiaozhuanlan.com")
	.scaleEffect(x: 1, y: 5)
/// 设置锚点
Text("swiftxiaozhuanlan.com")
	.scaleEffect(2, anchor: .bottomTrailing) 
```

