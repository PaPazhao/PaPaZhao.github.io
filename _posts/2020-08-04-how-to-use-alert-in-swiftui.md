---
title: 在 SwiftUI 中如何使用 Alert 弹窗
layout: post
date: 2020-08-04
---

通常在 UIKit 里系统弹窗使用 `UIAlertController` 来实现，而在 SwiftUI 中使用 Alert 来实现弹窗。

Alert 提供了两个初始化方法，区别是底部的按钮是一个还是两个。

```swift
/// 创建带有一个按钮的 Alert
init(title: Text, message: Text?, dismissButton: Alert.Button?)
/// 创建带有两个按钮的 Alert
init(title: Text, message: Text?, primaryButton: Alert.Button, secondaryButton: Alert.Button)
```

通过  Alert.Button 静态方法，可以创建 cancel，default 和 destructive 不同样式的功能按钮。

```swift
/// 取消按钮
static func cancel((() -> Void)?) -> Alert.Button 
/// 带 action 的取消按钮
static func cancel(Text, action: (() -> Void)?) -> Alert.Button 
/// 默认按钮
static func `default`(Text, action: (() -> Void)?) -> Alert.Button 
/// 具有破坏数据性质的按钮（红色）
static func destructive(Text, action: (() -> Void)?) -> Alert.Button 
```

创建 Alert 

```swift
Alert(
    title: Text("你已经完成购买")
)

Alert(
    title: Text("您的账户状态异常"),
    message: Text("请重新进行登录"),
    dismissButton: .default(Text("确定"))
)

Alert(
    title: Text("确认删除所有？"),
    message: Text("删除数据后无法恢复"),
    primaryButton: .cancel(),
    secondaryButton: .destructive(Text("删除"))
)
```

下面我们实现一个功能：在用户界面有个 "删除用户数据" 的按钮，在该按钮点击是弹窗提示用户是确认删除。

在 SwiftUI 中 Alert 是通过` func alert(isPresented: Binding<Bool>, content: () -> Alert) -> some View `附加到主视图上的，通过绑定主视图的一个的 Bool 状态值： `isPresented` 来表示弹窗是否展示。当需要弹窗时候修改 isPresented 为 true 即可。当按钮关闭时 isPresented 会自动被设置为 false

```swift
struct ContentView: View {
    // 1
    @State private var isPresented = false 
 
    var body: some View {
        Form {
            Button("删除用户数据") {
                isPresented.toggle() // 4
            }
            .alert(isPresented: $isPresented) { alert } // 3
        }
    }
    // 2
    private var alert: Alert {
        Alert(
            title: Text("确认删除用户数据？"),
            message: Text("删除数据后无法恢复"),
            primaryButton: .cancel(),
            secondaryButton: .destructive(Text("删除"), action: formateUserData)
        )
    }
    
    /// 格式化用户数据
    private func formateUserData() {}
}
```

## 总结

使用 Alert 弹窗的清单

1. 主视图定义 @State private var isPresented = false
2. 定义一个 alert 属性，创建一个 alert
3. 主视图调用 .alert(isPresented: $isPresented) {  some alert }
4. 主视图 isPresented 被修改为 true 时 alert 弹出
