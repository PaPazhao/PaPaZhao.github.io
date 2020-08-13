---
title: 在 SwiftUI 中如何实遮罩 mask view
---

SwiftUI 提供了对 view 进行遮罩 mask 的修饰符 **mask**，他的作用跟 CALayer 的maskLayer 一样都是实现一个图层对另一个图层进行遮罩。遮罩可以对任意的 view 进行任意形状的裁剪输出。

下面的代码实现了环形形状的文本。利用遮罩对大量的文本字符串进行遮罩 

```swift
let someString = "swiftxiaozhuanlan.com 这里是很长的字符串，此处略去10000字"

Text(someString)
    .lineLimit(100)
    .font(.caption2)
    .frame(width: 300, height: 300)
    .mask(
        Circle()
        	.strokeBorder(Color.blue, lineWidth: 40)
    )
```

下面的代码对一张图片使用环形胶囊进行遮罩

```swift
Image("mine_header_bg")
    .resizable()
    .frame(width: 300, height: 200)
    .mask(
        Capsule()
            .strokeBorder(Color.blue, lineWidth: 60, antialiased: true)
    )
```

