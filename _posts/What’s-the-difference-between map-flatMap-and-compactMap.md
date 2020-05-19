---
title: Swift 标准库里的三个 map、flatMap 和 compactMap 方法你知道有什么不同吗?
layout: post
date: 2020-05-14

---
# ![swift-tips](../public/swift-tips.png)

在 Swift 标准库里提供了 `map`、`flatMap` 和 `compactMap` 三个协议方法。他们都是函数式编程中用于对序列进行变换的方法，他们的用法看上去非常相似，但是他们有着不同的功能。因此本文详细聊一下 `map`、`flatMap` 和 `compactMap` 三个方法在不同的场景如何使用。

从命名上看，在这三个方法里具有相同的部分 `map` ，`map` 可以理解成映射、变换。他是对元素进行变型的方法，也就是可以把一个元素从 A 变成 B，A 和 B 可以是同一种类型，也可以是不同的类型。如果序列实现这一方法那么就可以对序列进行变换，把序列的所有元素使用同一方法进行变换。

比如可以对数组的元素进行数学运算：

```swift
let array = [1, 2, 3]
/// 对序列的元素进行平方操作, array 经过变换后变成 [1, 4, 9]
array.map { $0 * $0 }  

/// 对序列的元素进行乘2, array 经过变换后变成 [1, 4, 6]
array.map { $0 * 2 }
```

还可以调用数组的元素的方法来执行变换：

对序列进行遍历，然后通过 transform 闭包提供的方法对遍历取出的元素进行变换，生成一个新的值。  把一个序列转换成另一个序列，序列的长度保持不变。

```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]

/// 把 cast 数组里每个元素转换为小写， 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]
let lowercaseNames = cast.map { $0.lowercased() } 

/// 计算 cast 中每个元素的字符个数， 'letterCounts' == [6, 6, 3, 4]
let letterCounts = cast.map { $0.count } 
```

上面的两个例子中就使用数组的 `map` 方法来对数组进行变换，可以看出他对数组所有的元素通过调用相同的方法进行了变换。然而通常我们如果对一个数组所有的元素进行操作需要进行遍历，也就是我们常见的 `for-in`循环语句。然而这里并没有任何使用循环的蛛丝马迹，那么 `map` 方法到底是如何做到对数组进行遍历的呢？在谜底揭开之前我们先来了解一下 `指令式编程` 和 `声明式编程` 的基本概念。

## 指令式编程

指令式编程就是通过指令的方式告诉计算机如何去做，比如通过汇编要实现 `1 + 1 = 2` 这个功能就需要告诉计算机如何去执行。按照把大象装冰箱分几步的流程:

- 把要计算的数据从内存读取到 CPU 的加法器的寄存器
- 加法器执行加法操作
- 加法器把计算结果返回

可以看的出汇编语言需要精确的告诉计算机具体的执行步骤，而且需要开发者清楚的了解计算机的汇编指令集。由于汇编语言是和 CPU 相关的硬件语言，所以代码不具备可移植性。汇编语言之后出现了 C/C++ 这类的指令式语言，他解决了汇编语言不具备移植性的痛点，但是本质上他们都属于同一类编程范式：指令式编程。通常指令式编程范式都有循环、计算、跳转三种类型的语句，通过组合一系列的指令来指示计算机完成一定的功能。由于这样的计算机编程语言贴近计算机硬件，因此他的执行速率通常很高效。 下面通过一个具体的例子来详细说明一下。

比如有个学生考试系统，记录的学生的姓名和考试的成绩：

```swift
/// 学生
struct Student {
    /// 考试科目
    enum Subject: String, CaseIterable {
        case 语文, 数学, 英语, 体育
    }
    let name: String
    let scores: [Subject: Int]
}

let students = [
    Student(name: "王峰", scores: [.语文: 90, .数学: 98, .英语: 88, .体育: 70]),
    Student(name: "齐小天", scores: [.语文: 84, .数学: 74, .英语: 94, .体育: 80]),
    Student(name: "冯晓宇", scores: [.语文: 89, .数学: 62, .英语: 78, .体育: 95]),
    Student(name: "邹小兵", scores: [.语文: 77, .数学: 69, .英语: 80, .体育: 56]),
    Student(name: "姜彤", scores: [.语文: 90, .数学: 82, .英语: 78, .体育: 82]),
]
```

现在想要检查所有学生的平均分，并且输出第一名的姓名，通过使用计算、循环、跳转这些语句可以给出指令式编程的解决方案:

```swift
var bestTemp: (student: Student, totalScore: Float)?
for curStudent in students {
    var totalScore: Float = 0
    /// 通过遍历 scores，累计各个科目的成绩求总分
    for key in Subject.allCases {
        totalScore += curStudent.scores[key] ?? 0
    }
    /// 计算平均分
    let average = totalScore / Float(Subject.allCases.count)
    if let tmp = bestTemp {
        if average > tmp.totalScore {
            bestTemp = (curStudent, average)
        }
    } else {
        bestTemp = (curStudent, average)
    }
}
if let best = bestTemp {
    print("最高平均分: \(best.1), 姓名: \(best.0.name)")
} else {
    print("students 为空")
}
```

看到这样的方式编写的代码我们是无法一眼看出这些代码完成一个什么样的功能。而且随着代码行数的增多不但对我们阅读代码带来困难，同样可能会带来更多的 Bug，那么有什么方法来解决这个问题呢

## 声明式编程

指令式的编程是开发者告诉计算机一步一步的指令，计算机通过按部就班的执行来完成开发者代码意图。而声明式编程则不然，声明式编程并不关心计算机如何去做，他更关心的是告诉计算机我要做什么，很多具体的细节可以由编译器来和标准库来实现而不需要开发者来关心，开发者需要要描述需要计算机最终完成的结果。Swift 语言的函数式编程范式来实现声明式的编程范式。对于上面的例子，采用函数式的编程方式可以改写成:

```swift
/// 求平均分
var average: ([Subject: Float]) -> Float = { 
    $0.values.reduce(0, +) / Float(Subject.allCases.count) 
}
let bestStudent = students
    .map { (student: $0, average: average($0.scores)) } /// 1
    .sorted { $0.average > $1.average } /// 2
    .first /// 3
```

这段代码非常简短，而且很容易看成上面三行代码的意图：

1. 分别求每个学生的平均分，把学生成绩单的数组转换成平均分的数组
2. 按平均分对学生数组进行排序，高分在前
3. 取学生数组的第一个元素，就是平均分第一名的学生。

就这样简短的几行代码就完成了上面指令式编程例子里的代码功能，简短而且可读性非常高，开发者不需要关心底层如何对数组进行操作，只需要描述对数组如何进行变换。和指令式编程相比，声明式编程具有更级别的抽象，关注点从计算机转移到了开发者自身。指令式编程我们需要对任务进行细分，把需任务按照计算机的能够理解的方式拆解成需要计算机一步一步执行的指令，而声明式则不同，他更关注的是我要完成什么功能，因此从指令式编程到声明式编程难点在于思维方式的转变。

了解了指令式编程和函数式编程的基本概念，下面具体的来说一下 Swift 中 `map` 的具体用法

## Sequence 中 的 map

 Swift 的标准库提供了对于序列 `Collection` 有许多比如 map、sorted、filter 等常规的操作API。下面是 [标准库中 Sequence 的 map 方法的实现 ](https://github.com/apple/swift/blob/swift-4.0-branch/stdlib/public/core/Sequence.swift)。

```swift
/// Swift 标准库 Sequence 中 map 方法的实现
extension Sequence {
  @_inlineable
  public func map<T>(_ transform: (Element) throws -> T) rethrows -> [T] {
    let initialCapacity = underestimatedCount
    var result = ContiguousArray<T>()
    result.reserveCapacity(initialCapacity)

    var iterator = self.makeIterator()

    // Add elements up to the initial capacity without checking for regrowth.
    for _ in 0..<initialCapacity {
      result.append(try transform(iterator.next()!))
    }
    // Add remaining elements, if any.
    while let element = iterator.next() {
      result.append(try transform(element))
    }
    return Array(result)
  }
}
```

可以看出在标准库中把对序列进行遍历的操作封装在 map 内部，因为这部分功能是不需要开发者关心的，而需要开发者关心的仅仅是对序列元素如何变换，也就是 transform 闭包。

对应变换方法 transform 执行后转换的类型支持任意类型，所以转换后的序列元素可以是 `Optional` 类型。那么序列中就可能存在有 nil 的情况，然而我们只关心有值的情况，在这种情况下序列的 ni 就变得很烦人，因为后续的操作我们需要对元素进行解包判断，我们需要过滤掉 nil，此时你需要 `compactMap:` 方法

## **compactMap**:

下面是  [Swift 标准库中 Sequence 的 compactMap: 方法实现](https://github.com/apple/swift/blob/9ae451d3226bc51a86a07bda19e672ca57b7d155/stdlib/public/core/SequenceAlgorithms.swift) 

```swift
/// 通过 transform 对原序列进行变换，并且过滤掉 nil,  返回一个非空的序列。
extension Sequence {
    public func compactMap<ElementOfResult>(
        _ transform: (Element) throws -> ElementOfResult?
    ) rethrows -> [ElementOfResult] {
        return try _compactMap(transform)
    }
    
    public func _compactMap<ElementOfResult>(
        _ transform: (Element) throws -> ElementOfResult?
    ) rethrows -> [ElementOfResult] {
        var result: [ElementOfResult] = []
        for element in self {
            if let newElement = try transform(element) {
                result.append(newElement)
            }
        }
        return result
    }
}
```

通过源码可以看出 `compactMap` 是对原序列进行遍历，然后解包过滤掉 nil ，然后返回一个新的序列。

### 简单的示例:

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]
     
let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]
     
let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```

在 Swift 中返回 `Optional` 的情况非常常见，对于这样的情况使用 `compactMap:` 非常方便

## flatMap

`map` 和 `flatMap` 的不同主要在于 `transform` 方法。`map` 方法是由元素到元素的变换，而 `flatMap` 是由元素到序列的变化 `transform:(Element)  -> Sequence` 

```swift
/// 对序列元素进行转换后压平
extension Sequence {
  public func flatMap<SegmentOfResult: Sequence>(
    _ transform: (Element) throws -> SegmentOfResult
  ) rethrows -> [SegmentOfResult.Element] {
    var result: [SegmentOfResult.Element] = []
    for element in self {
      result.append(contentsOf: try transform(element))
    }
    return result
  }
}
```

### 简单的示例:

```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
// [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```

事实上 `s.flatMap(transform)`  相当于  `Array(s.map(transform).joined())`.

 

## Optional 的 map

`Optional ` 同样提供了 `map` 方法，仅仅在有值的情况下进行变换。下面是 [Swift 标准库 Optional 中 map 的实现](https://github.com/apple/swift/blob/master/stdlib/public/core/Optional.swift)


```swift
public func map<U>(_ transform: (Wrapped) throws -> U) 
	rethrows -> U? {
    switch self {
    case .some(let y):
      return .some(try transform(y))
    case .none:
      return .none
    }
}
```

### 简单的例子:

```swift
let possibleNumber: Int? = Int("42")
let possibleSquare = possibleNumber.map { $0 * $0 }
print(possibleSquare)
// Prints "Optional(1764)"

let noNumber: Int? = nil
let noSquare = noNumber.map { $0 * $0 }
print(noSquare)
// Prints "nil"
```



## Optional 的 flatMap

下面是 [Swift 标准库 Optional 中 flatMap 的实现](https://github.com/apple/swift/blob/master/stdlib/public/core/Optional.swift)

```swift
public func flatMap<U>(_ transform: (Wrapped) throws -> U?) 
	rethrows -> U? {
    switch self {
    case .some(let y):
      return try transform(y)
    case .none:
      return .none
    }
}
```

###  简单的例子:

```swift
let possibleNumber: Int? = Int("42")
let nonOverflowingSquare = possibleNumber.flatMap { x -> Int? in
    let (result, overflowed) = x.multipliedReportingOverflow(by: x)
    
    return overflowed ? nil : result
}
print(nonOverflowingSquare)
// Prints "Optional(1764)"
```

 Optional 的 `map` 方法和 `flatMap` 方法非常相似，但是有些不同。不同点在于 transform， map 的 transform 返回的是非可选，而 flatMap 返回的是可选

