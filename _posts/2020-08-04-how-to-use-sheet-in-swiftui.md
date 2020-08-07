---
title: 在 SwiftUI 中如何通过 modal sheet 的方式 present 一个 view
layout: post
date: 2020-08-04
---

在 UIKit 中我们通常调用 `UIViewController `  的 `present` 方法来 modal 一个控制器，而在 SwiftUI 中我们是通过调用 [sheet(isPresented:onDismiss:content:)](https://developer.apple.com/documentation/swiftui/anyview/sheet(ispresented:ondismiss:content:)) 方法来以 modal sheet 的方式 present 一个 View。

实现在邮箱中通过点击新建邮件的按钮来弹出一个写邮件的视图：CreateEmailView

在主视图邮箱中定义一个状态变量，

```swift
@State private var isPresented = false
```

在主视图调用 [sheet(isPresented:onDismiss:content:)](https://developer.apple.com/documentation/swiftui/anyview/sheet(ispresented:ondismiss:content:)) 方法来附加 CreateEmailView

```swift
.sheet(isPresented: $isPresented, content: CreateEmailView.init) 
```

在主视图点击创建新邮件的按钮时候需要 present，isPresented 的值为 true 时候 CreateEmailView 弹出

```swift
isPresented.toggle()
```

上面就是通过 modal sheet 的方式 present 一个view 的所有的流程。下面是本例中所有的代码

```swift
struct EmailBox: View {
    // 1
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
            .sheet(isPresented: $isPresented, content: CreateEmailView.init) // 3
        }
    }
    
    var createBarItem: some View {
        Image(systemName: "square.and.pencil")
            .foregroundColor(/*@START_MENU_TOKEN@*/.blue/*@END_MENU_TOKEN@*/)
            .onTapGesture {
                isPresented.toggle()  // 4
            }
    }
}
// 2
struct CreateEmailView: View {
    var body: some View {
        NavigationView {
            VStack {
                
            }.navigationTitle("新邮件")
        }
    }
}
```

## 总结

通过 modal sheet 的方式 present 视图 的使用清单：

1. 在主视图定义一个状态变量： `@State private var isPresented = false`

2. 定义要 present 的 view

3. 附加到主视图 `.sheet(isPresented: $isPresented, content: CreateEmailView.init)` 

4. 需要 present 的时候给 isPresented 赋值： isPresented = true

    

## 推荐阅读

- [在 SwiftUI 中如何使用 ActionSheet](http://swiftxiaozhuanlan.com/2020/08/04/how-to-use-actionsheet-in-swiftui/)

- [在 SwiftUI 中如何使用 Alert 弹窗](http://swiftxiaozhuanlan.com/2020/08/04/how-to-use-alert-in-swiftui/)

