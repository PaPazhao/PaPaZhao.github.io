---
title: 在 SwiftUI 中如何对 view 进行高斯模糊 blur
---

SwiftUI 提供了 blur 修饰符来实现对 view 的高斯模糊。

例如我们对一张图片进行模糊，模糊半径 10

```swift
Image("swiftxiaozhuanlan.com.icon")
    .resizable()
    .frame(width: 100, height: 100)
    .blur(radius: 10)
```

