---
title: 在 SwiftUI 中如何对 View 进行 3D 旋转 Rotation
layout: post
date: 2020-08-06
---

 我们之前介绍了 [在 SwiftUI 中如何对 View 进行 2D 旋转 Rotation](http://127.0.0.1:4000/2020/08/06/how-to-rotate-view/)，SwiftUI 使用 **rotation3DEffect** 修饰符对 View 进行 3D 旋转。

rotation3DEffect 的使用非常简单，只需要提供旋转的角度以及安装那个轴进行旋转两个参数就可以了。例如我们对 Text("swiftxiaozhuanlan.com") 以 x 为轴旋转45度，

```swift
Text("swiftxiaozhuanlan.com")
	.rotation3DEffect(.degrees(45), axis: (x: 1, y: 0, z: 0))
```

