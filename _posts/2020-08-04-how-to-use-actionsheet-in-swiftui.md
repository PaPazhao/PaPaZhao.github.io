---
title: 在 SwiftUI 中如何使用 ActionSheet
layout: post
date: 2020-08-04
---

之前的文章介绍了[在 SwiftUI 中如何使用 Alert 弹窗](http://swiftxiaozhuanlan.com/2020/08/04/how-to-use-alert-in-swiftui/)，这一篇介绍 在 SwiftUI 中如何使用 [ActionSheet](https://developer.apple.com/documentation/swiftui/actionsheet)。ActionSheet 和 Alert 工作方式类似，一母同胞都是弹窗供用户做选择的 `View`。

首先在主视图定义一个状态变量，`isPresented` 用来表示 `ActionSheet` 是否弹出。

```swift
@State private var isPresented = false 
```

定义一个 actionSheet 表示我们要弹窗的内容

```swift
private var actionSheet: ActionSheet {
    ActionSheet(
        title: Text("删除数据？"),
        message: Text("此操作将删除该App存储在当前iPhone和iCloud中的所有数据。"),
        buttons: [
            .destructive(Text("删除"), action: formateUserData),
            .cancel(),
        ]
    )
}
```

绑定到主视图

```swift
.actionSheet(isPresented: $isPresented) { actionSheet }
```

当需要弹窗时候设置  isPresented 的值为 `true`。完整的代码如下：

````swift
struct ActionSheetView: View {
    // 1
    @State private var isPresented = false
    var body: some View {
        Form {
            Button("删除用户数据") {
                isPresented.toggle() // 4
            }
            .actionSheet(isPresented: $isPresented) { actionSheet } // 3
        }
    }
    // 2
    private var actionSheet: ActionSheet {
        ActionSheet(
            title: Text("删除数据？"),
            message: Text("此操作将删除该App存储在当前iPhone和iCloud中的所有数据。"),
            buttons: [
                .destructive(Text("删除"), action: formateUserData),
                .cancel(),
            ]
        )
    }
    
    /// 格式化用户数据
    private func formateUserData() {}
}
````



## 总结：

ActionSheet 使用清单：

1. 在主视图定义一个 Bool 状态变量： `@State private var isPresented = false` 
2. 在主视图定义一个类型为 `ActionSheet` 的 `actionSheet` 计算属性
3. 通过 `.actionSheet(isPresented: $isPresented) { actionSheet }` 将 actionSheet 附加到主视图
4. 需要弹窗的时候设置: `isPresented = true`

