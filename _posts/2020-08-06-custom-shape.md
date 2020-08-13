---
title: 在 SwiftUI 中如何自定义绘制路径
---

SwiftUI 中绘制自定义的形状和 UIKit 非常的类似，都是在一个给定的区域内描绘你所需要绘制的路径。SwiftUI 中自定义形状需要遵守 **Shape** 协议即可。

在 Shape 协议中提供了一个 **func path(in rect: CGRect) -> Path** 函数，就是在矩形区域 rect 中描绘一个 **Path** 路径，这个路径就是我们自定义的 Shape。其实跟 **Path** 提供的 API 跟 **CGPath** 非常的类似，基本的操作步骤都是相同的。下面的代码描述了一个米字型的方格：

```swift
struct CustomShape: Shape {
    func path(in rect: CGRect) -> Path {
        var path = Path()
        path.addRect(rect)
        path.move(to: CGPoint(x: 0, y: rect.height * 0.5))
        path.addLine(to: CGPoint(x: rect.width, y: rect.height * 0.5))
        path.move(to: CGPoint(x: rect.width * 0.5, y: 0))
        path.addLine(to: CGPoint(x: rect.width * 0.5, y: rect.height))
        path.move(to: CGPoint(x: 0, y: rect.height))
        path.addLine(to: CGPoint(x: rect.width, y: 0))
        path.move(to: CGPoint(x: rect.width, y: rect.height))
        path.addLine(to: CGPoint(x: 0, y: 0))
        return path
    }
}
```

