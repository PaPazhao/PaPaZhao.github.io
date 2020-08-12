---
title: 在 SwiftUI 中如何对 View 进行 2D 旋转 Rotation
layout: post
date: 2020-08-06
---

SwiftUI 为我们提供了一个 2D 旋转 Rotation 视图的修饰符：**rotationEffect**，使用 **rotationEffect** 可以对 View 旋转对应的角度。

例如对文本 "swiftxiaozhuanlan.com" 旋转 30 度

```swift
Text("swiftxiaozhuanlan.com")
	.rotationEffect(.degrees(30))
```

默认情况围绕着 View 的中心旋转，但是可以指定旋转的锚点，例如

```sw
Text("swiftxiaozhuanlan.com")
	.rotationEffect(.degrees(30), anchor: .topLeading)
```

