---
title: 在 SwiftUI 中如何使用 clipShape 裁剪 View
layout: post
date: 2020-08-06
---

在 SwiftUI 中提供了一个裁剪视图形状的修饰符：**clipShape**，他可以将 View 以你自定义的任何 Shape 的形状进行裁剪。

例如我们有一个收藏按钮，可以使用 **clipShape** 将按钮按照 **Circle**、**Capsule**  的形状进行裁剪

```swift
/// 圆形按钮
Button(action: {}) {
    Image(systemName: "star")
        .foregroundColor(.white)
        .padding()
        .background(Color.yellow)
	    .clipShape(Circle())  
}
/// 圆角矩形按钮
Button(action: {}) {
    Image(systemName: "star")
        .foregroundColor(.white)
        .padding()
        .background(Color.yellow) 
	    .clipShape(RoundedRectangle(cornerRadius: 10.0))  
}
/// 胶囊类型
Button(action: {}) {
    Image(systemName: "star")
        .foregroundColor(.white)
        .frame(width: 100, height: 40)
        .background(Color.yellow) 
	    .clipShape(Capsule())  
}
```

