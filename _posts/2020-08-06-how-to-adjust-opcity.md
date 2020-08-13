---
title: 在 Swiftui 中如何设置视图的不透明度
---

SwiftUI 提供了 **opacity** 来修改 View 的不透明度，类似 UIView 的 alpha 属性。

给 Text("swiftxiaozhuanlan.com") 的背景设置 0.3 的不透明度。

```swift
Text("swiftxiaozhuanlan.com")
	.padding()
	.background(Color.green)
	.opacity(0.3)
```

