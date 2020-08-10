---
title: 在 SwiftUI 中如何 dissmiss 以 modal sheet 的方式 present 的 view
layout: post
date: 2020-08-04
---

上一篇文章介绍了[在 SwiftUI 中如何通过 modal sheet 的方式 present 一个 view](http://swiftxiaozhuanlan.com/2020/08/04/how-to-use-sheet-in-swiftui/)，SwiftUI 自动实现了在 view 弹出后通过下滑手势 dissmiss的方法，但是很多情况需要我们手动去触发 dismiss 的操作，比如邮件编辑完成后点击发送按钮后自动的 dismiss。那么本篇文章会接收如何 dismiss一个 presented 的 View 。

在 SwiftUI 中界面跟数据之间通过绑定的方式来响应改变，UI 跟数据之间可以进行双向的绑定，当数据发生改变的时候会触发绑定他的 UI 进行更新，UI 进行改变时会同步到其绑定数据，在上面一篇文章正是通过这种数据绑定的方式来 present 邮件的编辑界面 CreateEmailView。

```swift
/// 主视图
struct EmailBox: View {
    // 1
    @State private var isPresented = false
    var body: some View {
        NavigationView {
			 ....
            .sheet(isPresented: $isPresented, content: CreateEmailView.init)  // 2
        }
    }
}
```

1. isPresented 就是主视图中 Present 子视图的状态数据

2. isPresented 通过 .sheet(isPresented:) 方法被绑定到 UI 进而驱动 UI 的 present 和 dismiss

既然如此我们在 CreateEmailView 中自己 dismiss 自己的话是不是可以修改主视图中的 isPresented 参数来达到 dismiss的功能呢？答案是肯定的。实现的话非常简单，只需要将 isPresented 通过绑定的方式传递到 CreateEmailView ，然后在 CreateEmailView 内部来操作 isPresented 值即可实现在 CreateEmailView 内 dismiss 自己。

```swift

struct EmailBox: View {
    @State private var isPresented = false
    var body: some View {
        NavigationView {
            List {
                Text("发件箱")
                Text("收件箱")
                Text("草稿箱")
            }
            .navigationTitle("邮箱")
            .navigationBarItems(trailing: createBarItem)
            .sheet(isPresented: $isPresented, content: CreateEmailView.init)
        }
    }
    
    var createBarItem: some View {
        Image(systemName: "square.and.pencil")
            .foregroundColor(.blue)
            .onTapGesture {
                isPresented.toggle()
            }
    }
}

struct CreateEmailView: View {
    @Binding var isPresented: Bool
    var canceBarButton: some View {
        Button(action: { isPresented.toggle() }, label: {
            Image(systemName: "paperplane.fill")
        })
    }
    var body: some View {
        NavigationView {
            VStack {
                
            }.navigationTitle("新邮件")
            .navigationBarItems(trailing: canceBarButton)
        }
    }
}

```

这就是通过数据绑定实现自己 dismiss 自己的方式。这种方式必须在子视图和主视图直接通过数据绑定建立一个桥梁来实现数据通信，然而由于在开发中可能会遇到大量的页面需要 dismiss 自己，那么大量重复性的工作会变得让人烦躁，所以  SwiftUI 还提供了另一种方式实现同样的功能，那就是使用环境变量，把当前 view 的 presentation 状态绑定到状态变量中，所以我们只需要操作环境变量的 presentationMode 就可以实现 dismiss，这样子视图无需去绑定主视图的数据，即子视图无需关心是谁 present 的自己。

```swift
struct EmailBox: View {
    @State private var isPresented = false
    var body: some View {
        NavigationView {
            List {
                Text("发件箱")
                Text("收件箱")
                Text("草稿箱")
            }
            .navigationTitle("邮箱")
            .navigationBarItems(trailing: createBarItem)
            .sheet(isPresented: $isPresented) {
                CreateEmailView()
            }
        }
    }
    
    var createBarItem: some View {
        Image(systemName: "square.and.pencil")
            .foregroundColor(.blue)
            .onTapGesture {
                isPresented.toggle()
            }
    }
}

struct CreateEmailView: View {
    @Environment(\.presentationMode) var presentationMode
    var canceBarButton: some View {
        Button(action: {
            presentationMode.wrappedValue.dismiss()
        }, label: {
            Image(systemName: "paperplane.fill")
        })
    }
    var body: some View {
        NavigationView {
            VStack {
                
            }.navigationTitle("新邮件")
            .navigationBarItems(trailing: canceBarButton)
        }
    }
}
```

## 总结

View dismiss 自己的两种方式：

1. 通过子视图绑定主视图的 isPresented 状态，子视图自己操作 isPresented 实现自己 dismiss 自己
2. 子视图通过操作环境变量 presentationMode 的 dismiss 方法来 dismiss



