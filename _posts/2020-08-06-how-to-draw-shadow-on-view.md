---
title: 在 SwiftUI 中如何使用 .shadow 给 View 绘制阴影
layout: post
date: 2020-08-06
---

SwiftUI 提供了 **.shadow** 修饰符用来给 View 绘制阴影。 **.shadow** 提供了阴影的颜色、半径和位置偏移。

增加一个半径为 5 的的阴影，阴影会加在文本正下方。

```swift
Text("swiftxiaozhuanlan.com")
    .padding()
    .shadow(radius: 5)
```

还可以设置阴影的颜色

```swift
Text("swiftxiaozhuanlan.com")
    .padding()
    .shadow(color: .red, radius: 10)
```

设置阴影的位置：在 x、y 方向分别偏移 10

```swift
Text("swiftxiaozhuanlan.com")
    .padding()
    .shadow(color: .red, radius: 5, x: 10, y: 10)
```

增加一个边框，在边框的外面加阴影

```swift
Text("swiftxiaozhuanlan.com")
    .padding()
    .border(Color.green)
    .shadow(color: .red, radius: 5)
```

