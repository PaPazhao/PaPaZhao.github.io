---
title: 在 SwiftUI 中如何使用上下文菜单 contextMenu
layout: post
date: 2020-08-05
---

在 iOS 中使用 3D Touch 功能可以弹出一个上下文的菜单，比如在信息 APP 中我们长按一条信息会弹出一个菜单来对这条信息进行操作，然而 SwiftUI 提供了一个 modifier 来实现类似的功能：contextMenu。

```swift
public func contextMenu<MenuItems>(
    @ViewBuilder menuItems: () -> MenuItems
) -> some View where MenuItems : View
```
contextMenu 由一组按钮组成，MenuItems 遵守 View 协议 。

下面代码实现了一个消息列表 `MessageBox` ，在长按每一条消息的时候会弹出一个 contextMenu 菜单，菜单提供 Copy 或者 Delete 这条信息的菜单

```swift
struct MessageBox: View {
    @State private var messages = [
        "今天晚上行动", "明天你来我家好不", "不行明天我有事情，改天约"
    ]
    private func selectCopy(item: String) { 
    }
    private func selectDelete(item: String) { 
    }
    var body: some View {
        NavigationView {
            List {
                ForEach(messages, id: \.self) { item in
                    Text(item)
                        .contextMenu {
                            Button(action: {
                                selectCopy(item: item)
                            },
                            label: {
                                Text("Copy")
                            })
                            
                            Button(action: {
                                selectDelete(item: item)
                            }, label: {
                                Text("Delete")
                            })
                        }
                }
            }.navigationTitle("信息")
        }
    }
}
```

