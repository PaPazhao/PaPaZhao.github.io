---
title: 在 SwiftUI 中如何使用 fullScreenCover 来 modal 一个全屏的 View
layout: post
date: 2020-08-05
---

在 iOS13 中，使用 SwiftUI 来 modal 一个 View 只能以 sheet（[sheet(isPresented:onDismiss:content:)](https://developer.apple.com/documentation/swiftui/anyview/sheet(ispresented:ondismiss:content:))），具体的使用方法在之前的文章「 [在 SwiftUI 中如何通过 modal sheet 的方式 present 一个 view](http://swiftxiaozhuanlan.com/2020/08/04/how-to-use-sheet-in-swiftui/) 」介绍过，然而如果想以全屏的方式来 modal 只能使用 iOS 14 提供的 modifier [fullScreenCover(isPresented:onDismiss:content:)](https://developer.apple.com/documentation/swiftui/anyview/fullscreencover(ispresented:ondismiss:content:))。

fullScreenCover 的使用非常简单，跟 sheet(isPresented:onDismiss:content:) 一样。下面的代码就是 fullScreenCover 的简单使用示例。

```swift
struct FullScreenView: View {
    @State private var isPresented = false
    var body: some View {
        NavigationView {
            Button("Present!") {
                isPresented.toggle()
            }
            .fullScreenCover(isPresented: $isPresented, content: DetailView.init)
        }
    }
}

struct DetailView: View {
    @Environment(\.presentationMode) var presentationMode
    
    var body: some View {
        NavigationView {
            Button(action: {
                presentationMode.wrappedValue.dismiss()
            }, label: {
                Text("Dismiss")
             })
            .navigationTitle("Detail View")
        }
    }
}
```

