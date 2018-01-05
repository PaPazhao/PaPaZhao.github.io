---
layout: post
title: RawRepresentable
date: 2017-11-28 13:32:24.000000000 +09:00
tags: Apple文档
---

# 概述 

With a RawRepresentable type, you can switch back and forth between a custom type and an associated RawValue type without losing the value of the original RawRepresentable type.
> 使用RawRepresentable类型，您可以在自定义类型和关联的RawValue类型之间来回切换，而不会丢失原始RawRepresentable类型的值。

Using the raw value of a conforming type streamlines interoperation with Objective-C and legacy APIs and simplifies conformance to other protocols, such as Equatable, Comparable, and Hashable. 
> 使用符合类型的原始值简化了与Objective-C和传统API的互操作，并简化了与其他协议（例如Equatable，Comparable和Hashable）的一致性。

The RawRepresentable protocol is seen mainly in two categories of types: enumerations with raw value types and option sets.
> RawRepresentable 协议主要见于两类类型：具有原始值类型和选项集的枚举。 
 
 
## 带原始值的枚举类型 (Enumerations with Raw Values)
For any enumeration with a string, integer, or floating-point raw type, the Swift compiler automatically adds RawRepresentable conformance.
> 具有原始值的枚举对于具有字符串，整数或浮点原始类型的任何枚举，Swift编译器会自动添加RawRepresentable一致性。 

When defining your own custom enumeration, you give it a raw type by specifying the raw type as the first item in the enumeration’s type inheritance list. 
> 在定义自定义枚举时，通过将原始类型指定为枚举的类型继承列表中的第一项，将其指定为原始类型。

You can also use literals to specify values for one or more cases. For example, the Counter enumeration defined here has an Int raw value type and gives the first case a raw value of 1:

> 您也可以使用字面量来指定一个或多个case的值。例如，这里定义的Counter枚举具有一个Int原始值类型，并给出第一个case的原始值1：

Listing 1
```
enum Counter: Int {
    case one = 1, two, three, four, five
}
```
 
You can create a Counter instance from an integer value between 1 and 5 by using the `init?(rawValue:)` initializer declared in the RawRepresentable protocol.
> 您可以使用 `RawRepresentable` 协议中声明的 `init?(rawValue:)` 初始值设定项，从1到5之间的整数值创建 Counter 实例。
 
 This initializer is failable because although every case of the Counter type has a corresponding Int value, there are many Int values that don’t correspond to a case of Counter.
> 这个构造器是可失败构造器，因为虽然每个计数器类型的情况下有一个相应的诠释价值，有很多诠释值不符合一个case的计数器。
 
Listing 2
```
for i in 3...6 {
    print(Counter(rawValue: i))
}
// Prints "Optional(Counter.three)"
// Prints "Optional(Counter.four)"
// Prints "Optional(Counter.five)"
// Prints "nil"
```

## Option Sets
Option sets all conform to RawRepresentable by inheritance using the `OptionSet` protocol.
> 通过使用OptionSet协议的继承，选项集全都符合RawRepresentable。

Whether using an option set or creating your own, you use the raw value of an option set instance to store the instance’s bitfield.
> 无论使用选项集还是创建自己的选项集，都可以使用选项集实例的原始值来存储实例的位字段。

The raw value must therefore be of a type that conforms to the `FixedWidthInteger` protocol, such as UInt8 or Int. For example, the Direction type defines an option set for the four directions you can move in a game.
 > 原始值因此必须是符合 `FixedWidthInteger` 协议的类型，例如UInt8或Int。例如，“方向”类型为您在游戏中移动的四个方向定义了一个选项集。
 
Unlike enumerations, option sets provide a nonfailable `init(rawValue:)` initializer to convert from a raw value, because option sets don’t have an enumerated list of all possible cases. Option set values have a one-to-one correspondence with their associated raw values.
> 与枚举不同，选项集提供了一个不可失败的 `init（rawValue：）` 初始化器来转换原始值，因为选项集没有列举出所有可能的情况。 选项设置值与关联的原始值具有一一对应的关系。

In the case of the Directions option set, an instance can contain zero, one, or more of the four defined directions. This example declares a constant with three currently allowed moves. The raw value of the allowedMoves instance is the result of the bitwise OR of its three members’ raw values:
> 在设置“路线”选项的情况下，实例可以包含四个定义的方向中的零个，一个或多个。 这个例子用三个当前允许的移动来声明一个常量。 allowedMoves实例的原始值是其三个成员的原始值按位或的结果：

Listing 4

```
let allowedMoves: Directions = [.up, .down, .left]
print(allowedMoves.rawValue)
// Prints "7"
```

Option sets use bitwise operations on their associated raw values to implement their mathematical set operations. For example, the `contains()` method on allowedMoves performs a bitwise AND operation to check whether the option set contains an element.
> Option sets 对关联的原始值使用按位运算来实现其数学集操作。 例如 allowedMoves 上的 `contains()` 方法执行按位AND操作来检查选项集是否包含元素。

Listing 5

```
print(allowedMoves.contains(.right))
// Prints "false"
print(allowedMoves.rawValue & Directions.right.rawValue)
// Prints "0"
```

[原文](https://developer.apple.com/documentation/swift/rawrepresentable)

