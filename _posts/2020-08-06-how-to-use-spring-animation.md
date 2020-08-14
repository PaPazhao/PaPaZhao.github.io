---
title: 在 SwiftUI 中如何实现弹簧动画 spring
---

上一篇介绍了如何在 SwiftUI 中实现基础的动画，接下来使用 **Animation** 中的 **spring()** 方法实现一个弹性的动画效果。

下面的代码实现了一个操作：点击按钮对星星旋转60度，旋转过程采用 spring 动画。

```swift
VStack {
    Image(systemName: "star")
        .resizable()
        .frame(width: 300, height: 300)
        .foregroundColor(Color.red)
        .rotationEffect(.init(degrees: angle))
        .animation(.spring())

    Button("旋转 60 度") {
        angle += 60
    }
}
```

**spring** 的函数原型如下，可以对 spring 的参数进行调整：

持久的弹簧动画。当与其他动画`spring()`或同一属性上的动画混合时，每个动画将被其后继替换，从而保持从一个动画到下一个动画的速度。（可选）在一段时间内在弹簧之间混合响应值。`interactiveSpring()`

- blendDuration：插值持续的时间（秒）更改弹簧的响应值
- dampingFraction：阻尼系数，也就是弹簧在运动中收到阻力系数。值越大，阻力越大，弹簧越容易停下 应用为动画值的拖动量，作为产生临界阻尼所需量的估计值的一部分
- response：弹簧的弹性强度，值越大代表弹簧运动越缓慢。  零值要求无限刚度的弹簧，适合于驱动交互式动画

```swift
extension Animation {
    public static func spring(
        response: Double = 0.55, 
        dampingFraction: Double = 0.825, 
        blendDuration: Double = 0
	) -> Animation
 
	public static func interactiveSpring(
        response: Double = 0.15, 
        dampingFraction: Double = 0.86, 
        blendDuration: Double = 0.25
    ) -> Animation
}
```

