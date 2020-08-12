---
title: 在 SwiftUI 中如何给 Shape 绘制边框
layout: post
date: 2020-08-05
---

SwiftUI 中提供了用来绘制 2D 图层的 **Shape** 协议，而 **Shape** 提供了两个 modifier：**stroke** 和 **strokeBorder**  ，这两个 modifier 都可以用来给 **Shape** 绘制边框，但是他们也有着不同。接下来介绍这两个 modifier 的具体用法。

**stroke** 和 **strokeBorder** 都可以使用 **lineWidth** 设置边框的宽度，由于边框存在宽度，所以边框和 Shape 如何对齐就是 **stroke** 和 **strokeBorder** 的主要区别

- **strokeBorder** 边框的最外围和 Shape 的外边界对齐，也就是边框所有的内容处于 Shape 的内部。
- **stroke** 边框的中心线和 Shape 的外边界对齐，也就是有一半宽度的边框会突出在 Shape 的外面，而且超出 Shape 边界的边框不会影响整体 Shape 的 frame。超出边界的边框可能会遮挡其他 View 的内容。

下面的代码 VStack 中有两个相同的背景黑色，frame 宽高都是 200的 ZStack，而他们的边框宽度 50。运行代码可以清晰的看到，1号 ZStack 的边界和红色的边框的边界是对齐的，而2号 ZStack 的边框有一半的宽度超越了ZStack的边界并且覆盖到了1号 ZStack 。

```swift
VStack {
    ZStack{ // 1
        Rectangle().strokeBorder(Color.red.opacity(0.5), lineWidth: 50)
    }
        .background(Color.black)
        .frame(width:200, height: 200)

    ZStack{ // 2
        Rectangle().stroke(Color.green.opacity(0.5), lineWidth: 50)
     }
        .background(Color.black)
        .frame(width:200, height: 200)
}
```

