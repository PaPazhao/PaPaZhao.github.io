---
title: 在 SwiftUI 中如何给绑定（Binding）进行动画
---

在 SwiftUI 中，UI 是跟数据进行双向绑定的 ，UI 的变化依赖于他的 **Binding**，当绑定值发生变化时对应的 UI 也产生变化，那么如何在绑定值 **Binding** 发生变化的同时，UI 执行动画呢？**animation()**

为了实现 **Binding** 的变化时执行动画，**Binding** 扩展了一个 **animation()** 用于执行动画 

```swift
extension Binding { 
    /// 当绑定值发生变化时执行一个特定的动画
    /// Specifies an animation to perform when the binding value changes.
    ///
    /// - 参数 animation: 当绑定值发生变化时执行的特定的动画序列
    ///
    /// - 返回: 新的 Binding.
    public func animation(_ animation: Animation? = .default) -> Binding<Value>
}
```

使用非常简单，只需要给使用**animation()** 来给  **Binding** 指定一个动画序列即可。

例如系统 App 的通知设置页面，通知的一些设置必须在 **允许通知** 的 **Toggle** 按钮开启以后才会展示，那么 Toggle 在开关的过程中 UI 会刷新，在展示和隐藏的时候会有一个动画，而这个动画的执行是依赖一个绑定值 **notificationIsOpened** 这个值改变的时候会执行对应的展示或者隐藏的动画。

```swift
struct NotificationSetting: View {
    enum DisplayPreview: String, CaseIterable {
        case always
        case unlocked
        case never
    }
    
    @State private var notificationIsOpened = false
    @State private var selectedDisplayPreview = DisplayPreview.always
    
    var body: some View {
        NavigationView {
            List {
                Toggle("允许通知", isOn: $notificationIsOpened.animation())
                
                if notificationIsOpened {
                    Picker("显示预览", selection: $selectedDisplayPreview) {
                        ForEach(DisplayPreview.allCases, id: \.self) {
                            Text($0.rawValue)
                        }
                    }
                }
            }
            .listStyle(GroupedListStyle())
            .navigationBarTitle("通知", displayMode: .inline)
        }
    }
}
```

