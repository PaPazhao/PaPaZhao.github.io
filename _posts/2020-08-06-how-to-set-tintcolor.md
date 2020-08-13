---
title: 在 SwiftUI 中如何设置 TintColor
---

在 UIKit 中我们通常使用 tintColor 来给 UIView 着色，而在 SwiftUI 中通常使用 **accentColor** 修饰符实现同样的功能，他可以给 View 进行统一的着色达到主题风格一致的效果。

给 Slider、Button、Link 着色橘色

```swift
VStack {
    Slider(value: $value)
        .padding()
    Button("swiftxiaozhuanlan.com") {}
    	.padding()
    Link(
        "swiftxiaozhuanlan.com",
         destination: URL(string: "http://swiftxiaozhuanlan.com/")!
    )
}
    .accentColor(.orange)
```

