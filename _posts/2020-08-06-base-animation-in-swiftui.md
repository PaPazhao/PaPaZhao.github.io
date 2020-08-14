---
title: 在 SwiftUI 中实现基础的动画效果
---

SwiftUI 的提供了修饰符 **func animation(_ animation: Animation?) -> some View** 来实现基础动画。

例如按下按钮随机改变星星的不透明度

```swift
VStack {
    Image(systemName: "star")
        .resizable()
        .frame(width: 100, height: 100)
        .foregroundColor(Color.red)
        .opacity(opacityValue)
        .animation(.linear)

    Button("调整不透明度") {
        opacityValue = Double.random(in: 0..<1)
    }
}
```

可以看到星星的不透明度的变化有个线性的动画，可以把动画的时间设置的久一点，动画的过程看的更清楚一点

```swift
.animation(.linear(duration: 2))
```

**.animation** 修饰符需要提供一个 **Animation** 的参数，这个 **Animation** 以静态方法的方式提供了**easeIn**，**easeInOut**，**linear**，**spring**，**timingCure**，**interactiveSping** 等一系列的动画 API，后续我们会分别介绍这些具体的使用。



