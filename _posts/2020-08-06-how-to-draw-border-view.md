---
title: 在 SwiftUI 中如何给 View 绘制边框
layout: post
date: 2020-08-06
---

在 SwiftUI 中给 View 绘制边框绘制边框使用 **border** 修饰符，而且可以指定边框的宽度和拐角半径。

给 Text 增加一个边框：

```swift
Text("swiftxiaozhuan.com")
	.padding()
	.border(Color.red) // 默认宽度值是1p
```

边框的宽度设置为10：

```swift
Text("swiftxiaozhuan.com")
	.padding()
	.border(Color.red, width: 10)
```

边框支持多种样式，Color 是最基础的边框样式，只要支持 **ShapeStyle** 协议即可。以下是 SwiftUI 内置的边框样式： 

- AngularGradient
-  Color 
-  ForegroundStyle 
-   ImagePaint 
-   LinearGradient 
-   SeparatorShapeStyle 
-   SelectionShapeStyle 
-   RadialGradient

 **ForegroundStyle** 边框的颜色就是前景色：

```swift
Text("swiftxiaozhuan.com")
    .padding()
    .border(ForegroundStyle(), width: 10)
    .foregroundColor(.red)
```
使用线性渐变 LinearGradient 作为边框：
```swift
Text("swiftxiaozhuan.com")
    .padding()
    .border( 
        LinearGradient(gradient: Gradient(colors: [Color.red, Color.blue]), startPoint: .leading, endPoint: .trailing),
        width: 10
    )
    .foregroundColor(.red)
```

径向渐变 RadialGradient：

```swift
Text("swiftxiaozhuan.com")
    .padding()
    .border(
        RadialGradient(gradient: Gradient(colors: [Color.black, Color.green]), center: .center, startRadius: 5, endRadius: 130),
        width: 10)
    .foregroundColor(.red)
```

