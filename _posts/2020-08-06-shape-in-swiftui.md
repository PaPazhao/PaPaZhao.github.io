---
title: SwiftUI 中的 Shape
---

在 SwiftUI 中 **Shape** 协议是用来描述 2D 图层的协议，**Rectangle**、**RoundedRectangle**、**Capsule**、**Ellipse** 和 **Circle** 是 SwiftUI 提供的基础的 Shape。

```swift
// 100 * 100 的矩形
Rectangle()  
    .frame(width: 100, height: 100)

// 100 * 100 的圆角矩形，圆角半径25
RoundedRectangle(cornerRadius: 25)  
    .fill(Color.red)
    .frame(width: 100, height: 100)

// 100 * 50 的区域内画一个胶囊的形状
Capsule() 
    .fill(Color.green)
    .frame(width: 100, height: 50)

// 100 * 50 区域内画一个椭圆
Ellipse()  
    .fill(Color.blue)
    .frame(width: 100, height: 50)

// 100 * 50 区域内画一个圆
Circle() 
    .fill(Color.orange)
    .frame(width: 100, height: 50)
```

