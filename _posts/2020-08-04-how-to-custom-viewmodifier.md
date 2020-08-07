---
title: 在 SwiftUI 中如何自定义 ViewModifier
layout: post
date: 2020-08-04

---

SwiftUI 内置了大量的用来改变 `View` 样式的 modifier（遵守 [ViewModifier](https://developer.apple.com/documentation/swiftui/viewmodifier) 协议的特定的结构），通过一系列的 modifier 对 `View` 操作实现特定的样式，然而有时候我们需要对某一些不同类型的 `View` 实现相同的风格的操作，那么如何做到对这些 modifier 组合进行封装来实现特定功能的`ViewModifier` 呢？答案是：自定义 `ViewModifier`


## [ViewModifier](https://developer.apple.com/documentation/swiftui/viewmodifier) Protocol 是什么？


`ViewModifie` 是一个协议（Protocol）：一个可以用在 `View` 或者另一个 `view modifier` 上，用来产生一个不同于原始值的版本。也就是说 `ViewModifier` 是用来修改 `View` 或者 `view modifier` 并返回原始值的新版本。正是这种方式才可以对一个 `View` 进行一些列修改的的链式调用。



## ViewModifier 如何实现？

 `ViewModifier` 基本实现非常的简单，需要实现下面的方法：

```swift
func body(content: Self.Content) -> Self.Body
```

这个方法`body(content:)` 就是实现对原来的`View` 或 `view modifier`修改，并返会原始值的新版本的实现方法。参数 `content` 是原始值（需要被修改的`View`）的代理，返会值是一个新的`View` （原始值的新版本）。

例如在应用中一些 `View` 具有特定的样式，比如带圆角并且周围有阴影的背景等，我们创建一个遵守 `ViewModifier` 协议的结构体：RoundCornerShadowModifiers

```swift
struct RoundCornerShadowModifiers: ViewModifier {
    func body(content: Content) -> some View {
            content
            .font(.largeTitle)
            .foregroundColor(.white)
            .padding()
            .background(
                Color.red.cornerRadius(10)
                    .shadow(color: Color.black.opacity(0.6), radius: 20, x: 0.0, y: 0.0)
                    .blur(radius: 1)
            )
    }
}
```

现在我们通过 `view` 的 `.modifier()` 方法调用

```swift
Text("swiftxiaozhuanlan.com")
	.modifier(RoundCornerShadowModifiers())
```

通常我们使用 SwiftUI 的内置 modifier 时都通过 ` .font(.largeTitle)，.foregroundColor(.white) `的方式来调用的，这是种方式在 `View` 协议上扩展了对应的方法来实现的。下面我们来为我们的 `RoundCornerShadowModifiers` 实现一个对应的  `View`  扩展方法：

```swift
extension View {
    func roundCornerShadowStyle() -> some View {
        modifier(RoundCornerShadowModifiers())
    }
}
```

现在我们可以这样使用

```swift
Text("swiftxiaozhuanlan.com")
	.roundCornerShadowStyle()
```

如果需要自定义 `ViewModifier` 的参数也是可以的，例如指定 RoundCornerShadowModifiers 的圆角值和颜色，只需要增加两个参数即可: shadowColor 和 cornerRadius

```swift
struct RoundCornerShadowModifiers: ViewModifier {
    let shadowColor: Color
    let cornerRadius: CGFloat
 
    func body(content: Content) -> some View {
            content
            .font(.largeTitle)
            .foregroundColor(.white)
            .padding()
            .background(
                Color.red.cornerRadius(cornerRadius)
                    .shadow(color: shadowColor, radius: 20, x: 0.0, y: 0.0)
                    .blur(radius: 1)
            )
    }
}

extension View {
    func roundCornerShadowStyle(
        _ shadowColor: Color = Color.black.opacity(0.3),
        cornerRadius: CGFloat = 10
    ) -> some View {
        modifier(
            RoundCornerShadowModifiers(
                shadowColor: shadowColor,
                cornerRadius: cornerRadius
            )
        )
    }
}
```

我们就这样调用

```swift
Text("swiftxiaozhuanlan.com")
	.roundCornerShadowStyle(Color.blue.opacity(0.9), cornerRadius: 20)
```

















 

