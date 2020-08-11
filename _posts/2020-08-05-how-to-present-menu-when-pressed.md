---
title: 在 SwiftUI 中如何点击按钮弹出菜单
layout: post
date: 2020-08-05
---

在 iOS 14 中，SwiftUI 提供了一个菜单控件：[Menu](https://developer.apple.com/documentation/swiftui/menu)。可以使用简单的字符串或者自定义 View 来创建菜单的内容。

使用最简单的字符串作为 Menu 的样式

```swift
Menu("Actions") {
    Button("Duplicate", action: duplicate)
    Button("Rename", action: rename)
    Button("Delete…", action: delete)
}
```

同时 Menu 支持嵌套

```swift
Menu("Actions") {
    Button("Duplicate", action: duplicate)
    Button("Rename", action: rename)
    Button("Delete…", action: delete)
    Menu("Copy") {
        Button("Copy", action: copy)
        Button("Copy Formatted", action: copyFormatted)
        Button("Copy Library Path", action: copyPath)
    }
}
```
使用自定义的样式来创建的 Menu
```swift
Menu {
    Button("Open in Preview", action: openInPreview)
    Button("Save as PDF", action: saveAsPDF)
} label: {
    Image(systemName: "document")
    Text("PDF")
}
```

同时 Menu 提供了定义菜单样式的 modifier：menuStyle(_ :)

```swift
Menu("Editing") {
    Button("Set In Point", action: setInPoint)
    Button("Set Out Point", action: setOutPoint)
}
.menuStyle(EditingControlsMenuStyle())
```

