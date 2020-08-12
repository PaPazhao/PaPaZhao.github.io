---
title: 在 SwiftUI 中当 List 的 Row 被点击时如何 Push 到一个新的 View
layout: post
date: 2020-08-05
---

通常我们在列表 **UITableView** 或者 **UICollectionView** 中点击 Cell 来 Push 到一个新的页面会使用对应的代理方法 `didSelectRowAt`，然而在 SwiftUI 中没有对应的代理属性，也没有类似的可以 push 的API，那么如何在 SwiftUI 中 push 一个 View 呢？那就是使用 **NavigationLink**。

在 SwiftUI 中 **NavigationLink** 首先是一个 View，而且他可以控制导航 presentation，因此我们可以利用这个特性来实现当列表的某个Cell 被点击的时候可以 push 到一个新的页面。通常我们实现信息列表如下所示：

```swift
struct Message: Identifiable {
    var id = UUID()
    var content: String
    var number: String
}

struct MessageRow: View {
    var message: Message
    var body: some View {
        VStack(alignment: .leading) {
            Text(message.content)
                .font(.body)
            Text(message.number)
                .font(.callout)
        }
    }
}

struct MessageListView: View {
    private var messages = [
        Message(content: "下班了一起吃火锅吧！", number: "10080"),
        Message(content: "最近手头比较紧，能不能借我1块钱", number: "100s0"),
        Message(content: "你家的狗狗跑到我家里了", number: "102080"),
        Message(content: "哎，好烦啊！", number: "100280"),
    ]
    
    var body: some View {
        NavigationView {
            List(messages) { message in
                MessageRow(message: message)
            }
                .navigationTitle("信息")
        }
    }
}
```

 下面的 MessageDetail 是 Cell 点击后展示详细的信息视图， 

```swift
struct MessageDetail: View {
    var message: Message
    var body: some View {
        VStack {
            Text(message.content)
        }
            .navigationTitle("来自\(message.number)的消息")
    }
}
```

现在一切准备就绪，只需要实现点击跳转的功能了。那么我们需要做的就是把列表的 Cell 用 **NavigationLink** 包起来就能实现push的功能了。封装后 Cell 的代码如下

```swift
NavigationLink(
    destination: MessageDetail(message: message),
    label: {
        MessageRow(message: message)
    })
```

其实就是用 **NavigationLink** 把 **MessageDetail(message:)** 和 **MessageRow(message:)** 链接了起来，**NavigationLink** 收到点击的事件后执行了 push 操作。下面是完整的代码：

```swift
struct Message: Identifiable {
    var id = UUID()
    var content: String
    var number: String
}

struct MessageRow: View {
    var message: Message
    var body: some View {
        VStack(alignment: .leading) {
            Text(message.content)
                .font(.body)
            Text(message.number)
                .font(.callout)
        }
    }
}

struct MessageListView: View {
    private var messages = [
        Message(content: "下班了一起吃火锅吧！", number: "10080"),
        Message(content: "最近手头比较紧，能不能借我1块钱", number: "100s0"),
        Message(content: "你家的狗狗跑到我家里了", number: "102080"),
        Message(content: "哎，好烦啊！", number: "100280"),
    ]
    
    var body: some View {
        NavigationView {
            List(messages) { message in
                NavigationLink(
                    destination: MessageDetail(message: message),
                    label: {
                        MessageRow(message: message)
                    })
            }
                .navigationTitle("信息")
        }
    }
}
struct MessageDetail: View {
    var message: Message
    var body: some View {
        VStack {
            Text(message.content)
        }
            .navigationTitle("来自\(message.number)的消息")
    }
}
```

