---
layout: post
title: Equatable
date: 2018-01-05 14:32:24.000000000 +09:00
tags: Protocol翻译
---


# Equatable
A type that can be compared for value equality.  
一个可以进行值相等比较的类型

Types that conform to the `Equatable` protocol can be compared for equality using the equal-to operator (`==`) or inequality using the not-equal-to operator (`!=`). Most basic types in the Swift standard library conform to `Equatable`.  
符合 `Equatable` 协议的类型可以使用equal-to运算符（`==`）或使用不等于运算符（`！=`）的不等式进行比较。 Swift标准库中的大多数基本类型符合 `Equatable` 。  

Some sequence and collection operations can be used more simply when the elements conform to `Equatable`. For example, to check whether an array contains a particular value, you can pass the value itself to the `contains(_:)` method when the array's element conforms to `Equatable` instead of providing a closure that determines equivalence. The following example shows how the `contains(_:)` method can be used with an array of strings.  
当元素符合 `Equatable` 时，可以更简单地使用一些 `sequence` 和 `collection` 操作。例如，为了检查一个数组是否包含一个特定的值，当数组的元素符合 `Equatable` 时，可以将值本身传递给`contains（_ :)`方法，而不是提供一个确定等价的闭包。 下面的例子展示了如何将`contains（_ :)`方法与一个字符串数组一起使用。 

```
let students = ["Nora", "Fern", "Ryan", "Rainer"]

let nameToCheck = "Ryan"
if students.contains(nameToCheck) {
    print("\(nameToCheck) is signed up!")
} else {
    print("No record of \(nameToCheck).")
}
// Prints "Ryan is signed up!"
```

Conforming to the Equatable Protocol
====================================
Adding `Equatable` conformance to your custom types means that you can use more convenient APIs when searching for particular instances in a collection. `Equatable` is also the base protocol for the `Hashable` and `Comparable` protocols, which allow more uses of your custom type, such as constructing sets or sorting the elements of a collection. To adopt the `Equatable` protocol, implement the equal-to operator (`==`) as a static method of your type. The standard library provides an implementation for the not-equal-to operator (`!=`) for any `Equatable` type, which calls the custom `==` function and negates its result. As an example, consider a `StreetAddress` structure that holds the parts of a street address: a house or building number, the street name, and an optional unit number. Here's the initial declaration of the `StreetAddress` type:  
在您的自定义类型中添加 `Equatable` 意味着您可以在集合中搜索指定元素时使用更方便的API。 `Equatable` 也是`Hashable`和`Comparable`协议的基本协议，它允许更多地使用自定义类型，比如构造集合或者对集合中的元素进行排序。要采用 `Equatable` 协议，实现等于运算符（`==`）作为您的类型的静态方法。 标准库为任何 `Equatable` 类型提供了不等于运算符（`！=`）的实现，该类型调用自定义的`==`函数并对其结果取反。 例如，考虑一个"StreetAddress"结构，该结构包含街道地址的部分：房屋或建筑物号码，街道名称和可选的单元号码。

```
struct StreetAddress {
    let number: String
    let street: String
    let unit: String?

    init(_ number: String, _ street: String, unit: String? = nil) {
        self.number = number
        self.street = street
        self.unit = unit
    }
}
```

Now suppose you have an array of addresses that you need to check for a particular address. To use the `contains(_:)` method without including a closure in each call, extend the `StreetAddress` type to conform to `Equatable`.   
现在假设你有一组地址，你需要检查一个特定的地址。 要在每个调用中使用不包含闭包的`contains（_ :)`方法，请扩展`StreetAddress`类型以符合`Equatable`。

```
extension StreetAddress: Equatable {
    static func == (lhs: StreetAddress, rhs: StreetAddress) -> Bool {
        return
            lhs.number == rhs.number &&
            lhs.street == rhs.street &&
            lhs.unit == rhs.unit
    }
}
```

The `StreetAddress` type now conforms to `Equatable`. You can use `==` to check for equality between any two instances or call the `Equatable`-constrained `contains(_:)` method.  
`StreetAddress` 类型现在符合 `Equatable` 。 你可以用==来检查任何两个实例之间的相等性，或者调用`Equatable`-constrained`contains（_ :)`方法。

```
let addresses = [StreetAddress("1490", "Grove Street"),
                 StreetAddress("2119", "Maple Avenue"),
                 StreetAddress("1400", "16th Street")]
let home = StreetAddress("1400", "16th Street")

print(addresses[0] == home)
// Prints "false"
print(addresses.contains(home))
// Prints "true"
```

Equality implies substitutability---any two instances that compare equally can be used interchangeably in any code that depends on their values. To maintain substitutability, the `==` operator should take into account all visible aspects of an `Equatable` type. Exposing nonvalue aspects of `Equatable` types other than class identity is discouraged, and any that are exposed should be explicitly pointed out in documentation.   
相等意味着可替代性 - 任何两个相同比较的实例可以在任何依赖于它们的值的代码中互换使用。 为了保持可替换性，`==`运算符应该考虑 `Equatable` 类型的所有可见方面。 暴露类型标识以外的 `Equatable` 类型的非值方面是不鼓励的，任何暴露的*都应该在文档中明确指出。  

Since equality between instances of `Equatable` types is an equivalence relation, any of your custom types that conform to `Equatable` must satisfy three conditions, for any values `a`, `b`, and `c`: 
由于 `Equatable` 类型的实例之间的等价关系是等价关系，所以任何符合 `Equatable` 的自定义类型都必须满足三个条件：a，b，c。 
 
- `a == a` is always `true` (Reflexivity)
- `a == b` implies `b == a` (Symmetry)
- `a == b` and `b == c` implies `a == c` (Transitivity)

- "a == a"总是"真"（反身性）
- `a == b`意味着`b == a`（对称）
- `a == b`和`b == c`意味着`a == c`（传递性）


Moreover, inequality is the inverse of equality, so any custom implementation of the `!=` operator must guarantee that `a != b` implies `!(a == b)`. The default implementation of the `!=` operator function satisfies this requirement.   
而且，不等式是相等的倒数，所以`！=`操作符的任何自定义实现都必须保证`a！= b`意味着`！（a == b）`。 `！=`运算符函数的缺省实现满足这个要求。

Equality is Separate From Identity
----------------------------------

The identity of a class instance is not part of an instance's value. Consider a class called `IntegerRef` that wraps an integer value. Here's the definition for `IntegerRef` and the `==` function that makes it conform to `Equatable`:
类实例的恒等式不是实例的一部分。 考虑一个名为`IntegerRef`的类，它封装了一个整数值。 这里是`IntegerRef`和`==`函数的定义，使它符合`Equatable`：

```
class IntegerRef: Equatable {
    let value: Int
    init(_ value: Int) {
        self.value = value
    }

    static func == (lhs: IntegerRef, rhs: IntegerRef) -> Bool {
        return lhs.value == rhs.value
    }
}
```

The implementation of the `==` function returns the same value whether its two arguments are the same instance or are two different instances with the same integer stored in their `value` properties. For example:   
无论其两个参数是相同的实例，还是两个不同的实例，在它们的`value`属性中存储相同的整数，`==`函数的实现返回相同的值。 例如：

```
let a = IntegerRef(100)
let b = IntegerRef(100)

print(a == a, a == b, separator: ", ")
// Prints "true, true"
```

Class instance identity, on the other hand, is compared using the triple-equals identical-to operator (`===`). For example:  
另一方面，类实例恒等式使用三等号等于相同的运算符（`===`）进行比较。 例如：

```
let c = a
print(a === c, b === c, separator: ", ")
// Prints "true, false"
```

```
public protocol Equatable {
    
    /// Returns a Boolean value indicating whether two values are equal.
    
    /// Equality is the inverse of inequality. For any values `a` and `b`,
    /// `a == b` implies that `a != b` is `false`.
    /// 
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func ==(lhs: Self, rhs: Self) -> Bool
}

extension Equatable {
    
    /// Returns a Boolean value indicating whether two values are not equal.
    
    /// Inequality is the inverse of equality. For any values `a` and `b`, `a != b`
    /// implies that `a == b` is `false`.
    
    /// This is the default implementation of the not-equal-to operator (`!=`)
    /// for any type that conforms to `Equatable`.
    
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func !=(lhs: Self, rhs: Self) -> Bool
}
```

Returns a Boolean value indicating whether two references point to the same object instance.
返回一个布尔值，指示两个引用是否指向相同的对象实例。

This operator tests whether two instances have the same identity, not the same value. For value equality, see the equal-to operator (`==`) and the `Equatable` protocol.
该运算符测试两个实例是否具有相同的标识，而不是相同的值。 对于值相等，请参阅equal-to运算符（`==`）和`Equatable`协议。

The following example defines an `IntegerRef` type, an integer type with reference semantics.
下面的例子定义了一个`IntegerRef`类型，一个带引用语义的整数类型。

```
class IntegerRef: Equatable {
    let value: Int
    init(_ value: Int) {
        self.value = value
    }
}

func ==(lhs: IntegerRef, rhs: IntegerRef) -> Bool {
    return lhs.value == rhs.value
}
```

Because `IntegerRef` is a class, its instances can be compared using the identical-to operator (`===`). In addition, because `IntegerRef` conforms to the `Equatable` protocol, instances can also be compared using the equal-to operator (`==`).
因为`IntegerRef`是一个类，它的实例可以使用相同的运算符（`===`）进行比较。 另外，由于`IntegerRef`符合`Equatable`协议，因此也可以使用等于运算符（`==`）比较实例。

```
let a = IntegerRef(10)
let b = a
print(a == b)
// Prints "true"
print(a === b)
// Prints "true"
```

The identical-to operator (`===`) returns `false` when comparing two references to different object instances, even if the two instances have the same value. 
比较两个引用到不同的对象实例时，相同的运算符（`===`）返回`false`，即使两个实例具有相同的值。

```
let c = IntegerRef(10)
print(a == c)
// Prints "true"
print(a === c)
// Prints "false"
```
```
/// - Parameters:
///   - lhs: A reference to compare.
///   - rhs: Another reference to compare.
public func ===(lhs: Swift.AnyObject?, rhs: Swift.AnyObject?) -> Bool
```
Returns a Boolean value indicating whether two references point to different object instances.
返回一个布尔值，指示两个引用是否指向不同的对象实例。

This operator tests whether two instances have different identities, not different values. For value inequality, see the not-equal-to operator (`!=`) and the `Equatable` protocol. 
该运算符测试两个实例是否具有不同的身份，而不是不同的值。 对于值不等式，请参阅不等于运算符（`！=`）和 `Equatable` 协议。

```
- Parameters:
  - lhs: A reference to compare.
  - rhs: Another reference to compare.
public func !==(lhs: Swift.AnyObject?, rhs: Swift.AnyObject?) -> Bool
```