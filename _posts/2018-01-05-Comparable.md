---
layout: post
title: Comparable
date: 2018-01-05 17:32:24.000000000 +09:00
tags: Protocol翻译
---


# [Comparable](https://github.com/apple/swift/blob/master/stdlib/public/core/Comparable.swift)
A type that can be compared using the relational operators `<`, `<=`, `>=`, and `>`.  
可以使用关系运算符`<`，`<=`，`> =`和`>进行比较的类型。  

The `Comparable` protocol is used for types that have an inherent order, such as numbers and strings. Many types in the standard library already conform to the `Comparable` protocol. Add `Comparable` conformance to your own custom types when you want to be able to compare instances using relational operators or use standard library methods that are designed for `Comparable` types.    
“Comparable”协议用于具有固有顺序的类型，如数字和字符串。 标准库中的许多类型已经符合“Comparable”协议。 当您希望能够使用关系运算符比较实例或使用为“Comparable”类型设计的标准库方法时，将“Comparable”一致性添加到您自己的自定义类型中。  

The most familiar use of relational operators is to compare numbers, as in the following example:  
关系运算符最常见的用法是比较数字，如下例所示：  

```
let currentTemp = 73

if currentTemp >= 90 {
    print(“It’s a scorcher!”)
} else if currentTemp < 65 {
    print(“Might need a sweater today.”)
} else {
    print(“Seems like picnic weather!”)
}
// Prints “Seems like picnic weather!”
```  

You can use special versions of some sequence and collection operations when working with a `Comparable` type. For example, if your array's elements conform to `Comparable`, you can call the `sort()` method without using arguments to sort the elements of your array in ascending order.   
使用“Comparable”类型时，可以使用某些序列和集合操作的特殊版本。 例如，如果数组的元素符合“Comparable”，则可以调用sort（）方法，而无需使用参数按升序对数组的元素进行排序。  

```  
var measurements = [1.1, 1.5, 2.9, 1.2, 1.5, 1.3, 1.2]
measurements.sort()
print(measurements)
// Prints “[1.1, 1.2, 1.2, 1.3, 1.5, 1.5, 2.9]”
```  

Conforming to the Comparable Protocol
=====================================

Types with Comparable conformance implement the less-than operator (`<`) and the equal-to operator (`==`). These two operations impose a strict total order on the values of a type, in which exactly one of the following must be true for any two values `a` and `b`:   
具有可比较一致性的类型实现了小于运算符（`<`）和等于运算符（`==`）。 这两个操作对一个类型的值施加严格的总体顺序，其中对于任何两个值“a”和“b”，下列中的一个必须是正确的：  

- `a == b`
- `a < b`
- `b < a`
 

In addition, the following conditions must hold:  
另外，下列条件必须成立：

- `a < a` is always `false` (Irreflexivity) // 反射性
- `a < b` implies `!(b < a)` (Asymmetry) // 不对称性
- `a < b` and `b < c` implies `a < c` (Transitivity) // 传递性

To add `Comparable` conformance to your custom types, define the `<` and `==` operators as static methods of your types. The `==` operator is a requirement of the `Equatable` protocol, which `Comparable` extends---see that protocol's documentation for more information about equality in Swift. Because default implementations of the remainder of the relational operators are provided by the standard library, you'll be able to use `!=`, `>`, `<=`, and `>=` with instances of your type without any further code.  
为了在你的自定义类型中遵守“Comparable”，把`<`和`==`运算符定义为你的类型的静态方法。 “==”运算符是“Equatable”协议的要求，“Comparable”扩展了---请参阅该协议的文档，了解有关Swift中的相等性的更多信息。 因为关系运算符的其余部分的默认实现是由标准库提供的，所以你可以使用`！=`，`>`，`<=`和`> = 进一步的代码

As an example, here's an implementation of a `Date` structure that stores the year, month, and day of a date:  
作为一个例子，下面是一个存储日期的年，月，日的“Date”结构的实现：

```
    struct Date {
        let year: Int
        let month: Int
        let day: Int
    }
```

To add `Comparable` conformance to `Date`, first declare conformance to `Comparable` and implement the `<` operator function.   
为了在Date中添加Comparable，首先声明符合Comparable，并实现`<`操作符函数。  

```
extension Date: Comparable {
    static func < (lhs: Date, rhs: Date) -> Bool {
        if lhs.year != rhs.year {
            return lhs.year < rhs.year
        } else if lhs.month != rhs.month {
            return lhs.month < rhs.month
        } else {
            return lhs.day < rhs.day
        }
    }
```
This function uses the least specific nonmatching property of the date to determine the result of the comparison. For example, if the two `year` properties are equal but the two `month` properties are not, the date with the lesser value for `month` is the lesser of the two dates.  Next, implement the `==` operator function, the requirement inherited from the `Equatable` protocol.  

```
        static func == (lhs: Date, rhs: Date) -> Bool {
            return lhs.year == rhs.year && lhs.month == rhs.month
                && lhs.day == rhs.day
        }
    }
```

Two `Date` instances are equal if each of their corresponding properties is
equal.

Now that `Date` conforms to `Comparable`, you can compare instances of the
type with any of the relational operators. The following example compares
the date of the first moon landing with the release of David Bowie's song
"Space Oddity":

```
    let spaceOddity = Date(year: 1969, month: 7, day: 11)   // July 11, 1969
    let moonLanding = Date(year: 1969, month: 7, day: 20)   // July 20, 1969
    if moonLanding > spaceOddity {
        print("Major Tom stepped through the door first.")
    } else {
        print("David Bowie was following in Neil Armstrong's footsteps.")
    }
    // Prints "Major Tom stepped through the door first."
```
Note that the `>` operator provided by the standard library is used in this
example, not the `<` operator implemented above.

- Note: A conforming type may contain a subset of values which are treated as exceptional---that is,   
注意: 符合类型可能包含被视为例外的值的子集，也就是说，  
values that are outside the domain of meaningful arguments for the purposes of the `Comparable` protocol.   
对于“Comparable”协议而言，这些值在有意义的参数的范围之外。 

For example, the special "not a number" value for floating-point types (`FloatingPoint.nan`) compares as neither less than, greater than, nor equal to any normal floating-point value. Exceptional values need not take part in the strict total order.  
例如，对于浮点类型（“FloatingPoint.nan”）的特殊“非数字”值（不是小于，大于或等于任何正常的浮点值）进行比较。 例外的价值观不需要参与严格的总体秩序。

```
public protocol Comparable : Equatable {

    /// Returns a Boolean value indicating whether the value of the first
    /// argument is less than that of the second argument.
    ///
    /// This function is the only requirement of the `Comparable` protocol. The
    /// remainder of the relational operator functions are implemented by the
    /// standard library for any type that conforms to `Comparable`.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func <(lhs: Self, rhs: Self) -> Bool

    /// Returns a Boolean value indicating whether the value of the first
    /// argument is less than or equal to that of the second argument.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func <=(lhs: Self, rhs: Self) -> Bool

    /// Returns a Boolean value indicating whether the value of the first
    /// argument is greater than or equal to that of the second argument.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func >=(lhs: Self, rhs: Self) -> Bool

    /// Returns a Boolean value indicating whether the value of the first
    /// argument is greater than that of the second argument.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func >(lhs: Self, rhs: Self) -> Bool
}

extension Comparable {

    /// Returns a Boolean value indicating whether the value of the first argument
    /// is greater than that of the second argument.
    ///
    /// This is the default implementation of the greater-than operator (`>`) for
    /// any type that conforms to `Comparable`.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func >(lhs: Self, rhs: Self) -> Bool

    /// Returns a Boolean value indicating whether the value of the first argument
    /// is less than or equal to that of the second argument.
    ///
    /// This is the default implementation of the less-than-or-equal-to
    /// operator (`<=`) for any type that conforms to `Comparable`.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    public static func <=(lhs: Self, rhs: Self) -> Bool

    /// Returns a Boolean value indicating whether the value of the first argument
    /// is greater than or equal to that of the second argument.
    ///
    /// This is the default implementation of the greater-than-or-equal-to operator
    /// (`>=`) for any type that conforms to `Comparable`.
    ///
    /// - Parameters:
    ///   - lhs: A value to compare.
    ///   - rhs: Another value to compare.
    /// - Returns: `true` if `lhs` is greater than or equal to `rhs`; otherwise,
    ///   `false`.
    public static func >=(lhs: Self, rhs: Self) -> Bool

    /// Returns a partial range up to, but not including, its upper bound.
    ///
    /// Use the prefix half-open range operator (prefix `..<`) to create a
    /// partial range of any type that conforms to the `Comparable` protocol.
    /// This example creates a `PartialRangeUpTo<Double>` instance that includes
    /// any value less than `5.0`.
    ///
    ///     let upToFive = ..<5.0
    ///
    ///     upToFive.contains(3.14)       // true
    ///     upToFive.contains(6.28)       // false
    ///     upToFive.contains(5.0)        // false
    ///
    /// You can use this type of partial range of a collection's indices to
    /// represent the range from the start of the collection up to, but not
    /// including, the partial range's upper bound.
    ///
    ///     let numbers = [10, 20, 30, 40, 50, 60, 70]
    ///     print(numbers[..<3])
    ///     // Prints "[10, 20, 30]"
    ///
    /// - Parameter maximum: The upper bound for the range.
    prefix public static func ..<(maximum: Self) -> PartialRangeUpTo<Self>

    /// Returns a partial range up to, and including, its upper bound.
    ///
    /// Use the prefix closed range operator (prefix `...`) to create a partial
    /// range of any type that conforms to the `Comparable` protocol. This
    /// example creates a `PartialRangeThrough<Double>` instance that includes
    /// any value less than or equal to `5.0`.
    ///
    ///     let throughFive = ...5.0
    ///
    ///     throughFive.contains(4.0)     // true
    ///     throughFive.contains(5.0)     // true
    ///     throughFive.contains(6.0)     // false
    ///
    /// You can use this type of partial range of a collection's indices to
    /// represent the range from the start of the collection up to, and
    /// including, the partial range's upper bound.
    ///
    ///     let numbers = [10, 20, 30, 40, 50, 60, 70]
    ///     print(numbers[...3])
    ///     // Prints "[10, 20, 30, 40]"
    ///
    /// - Parameter maximum: The upper bound for the range.
    prefix public static func ...(maximum: Self) -> PartialRangeThrough<Self>

    /// Returns a partial range extending upward from a lower bound.
    ///
    /// Use the postfix range operator (postfix `...`) to create a partial range
    /// of any type that conforms to the `Comparable` protocol. This example
    /// creates a `PartialRangeFrom<Double>` instance that includes any value
    /// greater than or equal to `5.0`.
    ///
    ///     let atLeastFive = 5.0...
    ///
    ///     atLeastFive.contains(4.0)     // false
    ///     atLeastFive.contains(5.0)     // true
    ///     atLeastFive.contains(6.0)     // true
    ///
    /// You can use this type of partial range of a collection's indices to
    /// represent the range from the partial range's lower bound up to the end
    /// of the collection.
    ///
    ///     let numbers = [10, 20, 30, 40, 50, 60, 70]
    ///     print(numbers[3...])
    ///     // Prints "[40, 50, 60, 70]"
    ///
    /// - Parameter minimum: The lower bound for the range.
    postfix public static func ...(minimum: Self) -> PartialRangeFrom<Self>
}
```