---
title: 在 SwiftUI 中如何在一个 View 上面覆盖另一个 View
layout: post
date: 2020-08-06
---

SwiftUI 提供了修饰符 overlay 来实现在一个 View 前面覆盖放置另一个 View

```swift
Image(systemName: "folder")
    .font(.system(size: 55, weight: .thin))
    .overlay(Text("❤️"), alignment: .bottom)
```

