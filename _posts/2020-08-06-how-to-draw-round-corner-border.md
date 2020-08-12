---
title: 在 SwiftUI 中如何给 View 添加带圆角矩形的边框
layout: post
date: 2020-08-06
---

给 View 添加边框非常简单，使用修饰符 border 即可，具体的使用方式[在 SwiftUI 中如何给 View 绘制边框](http://swiftxiaozhuanlan.com/2020/08/06/how-to-draw-border-view/) 这篇文章中介绍过，然而如何绘制一个带圆角的边框呢？

通常我们会这样思考：圆角矩形边框需要两步操作那就是先给 view 使用 border 加上边框，然后再用 cornerRadius 切出圆角即可。代码如下

```swift
Text("swiftxiaozhuan.com")
    .padding()
    .border(Color.red, width: 10)
    .cornerRadius(5)
    .foregroundColor(.red)
```

我们实现了边框宽度为 10，圆角为 5 的带圆角的边框。然而当我们把圆角设置比较大的时候比如 30 就会发现问题。这种实现方式 cornerRadius 只是将绘制边框后整个 View 四个角裁剪了以 5 半径的角，而并非是边框本身就是圆角的。

其实 SwiftUI 提供了圆角矩形的 Shape： **RoundedRectangle** ，我们只需要在这个形状上使用 stroke 绘制需要的边框，这样圆角边框就做好了，然后将这个做好的边框图层通过修饰符 overlay 覆盖到 View 上即可。

关于如何在 Shape 上绘制边框看这篇文章：[在 SwiftUI 中如何给 Shape 绘制边框](http://swiftxiaozhuanlan.com/2020/08/05/how-to-draw-border-shape/) 就可以了。

```swift
Text("swiftxiaozhuan.com")
    .padding() 
    .foregroundColor(.red)
    .overlay(
        RoundedRectangle(cornerRadius: 10)
            .stroke(Color.red, lineWidth: 4)
    )
```



