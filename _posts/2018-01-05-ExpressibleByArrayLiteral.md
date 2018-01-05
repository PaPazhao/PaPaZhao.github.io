---
layout: post
title: ExpressibleByArrayLiteral
date: 2018-01-05 13:32:24.000000000 +09:00
tags: Protocol翻译
---


# ExpressibleByArrayLiteral
  
A type that can be initialized using an array literal.  
可以使用字面量数组初始化的类型

An array literal is a simple way of expressing a list of values. Simply surround a comma-separated list of values, instances, or literals with square brackets to create an array literal. You can use an array literal anywhere an instance of an `ExpressibleByArrayLiteral` type is expected: as a value assigned to a variable or constant, as a parameter to a method or initializer, or even as the subject of a nonmutating operation like `map(_:)` or `filter(_:)`.    
字面量数组是表达一个值列表的一种简单的方式。简单的用逗号分隔列表值，实例或者字面量，然后用方括号括起=来就可以创建字面量数组。你可以使用字面量数组在任意符合`ExpressibleByArrayLiteral`类型：作为分配给常量或者变量的值；作为方法或者初始化器的参数；甚至作为像`map(_:)` 或者 `filter(_:)`这样的非变形操作（nonmutating operation）的对象
 
Arrays, sets, and option sets all conform to `ExpressibleByArrayLiteral`,
and your own custom types can as well. Here's an example of creating a set
and an array using array literals:
Array,Set,或者可选Set以及所有自定义符合 `ExpressibleByArrayLiteral`的类型都可以。这里有一个使用字母数组创建一个set和Array的例子。

```
let employeesSet: Set<String> = ["Amir", "Jihye", "Dave", "Alessia", "Dave"]
print(employeesSet)
// Prints "["Amir", "Dave", "Jihye", "Alessia"]"

let employeesArray: [String] = ["Amir", "Jihye", "Dave", "Alessia", "Dave"]
print(employeesArray)
// Prints "["Amir", "Jihye", "Dave", "Alessia", "Dave"]"
```

The `Set` and `Array` types each handle array literals in their own way to
create new instances. In this case, the newly created set drops the duplicate value ("Dave") and doesn't maintain the order of the array literal's elements. The new array, on the other hand, matches the order and number of elements provided.
Set和Array类型都有自己的方式处理字面量数组去创建新实例。在这个例子中新创建的Set会丢弃重复值("Dave”)并且不会维护字面量数组的元素顺序。另一方面，新数组匹配元素的顺序和数量。

- Note: An array literal is not the same as an `Array` instance. You can't
initialize a type that conforms to `ExpressibleByArrayLiteral` simply by
assigning an existing array.  
注意： 字面量数组不等同于数组实例。你不能简单的使用一个已经存在的数组来初始化一个遵守`ExpressibleByArrayLiteral`协议类型实例。

```
let anotherSet: Set = employeesArray
// error: cannot convert value of type '[String]' to specified type 'Set'
```


字面量数组的类型推断（Type Inference of Array Literals  ）
================================

Whenever possible, Swift's compiler infers the full intended type of your array literal. Because `Array` is the default type for an array literal, without writing any other code, you can declare an array with a particular element type by providing one or more values.  
只要有可能，Swift的编译器就会推断你字面量数组的完整预期类型。由于“Array”是字面量数组的默认类型，因此可以通过提供一个或多个值来声明具有特定元素类型的数组，并不需要编写任何其他代码，。

In this example, the compiler infers the full type of each array literal.   
在这个例子中，编译器推断出每个字面量数组的全类型。

```
let integers = [1, 2, 3]
// 'integers' has type '[Int]'

let strings = ["a", "b", "c"]
// 'strings' has type '[String]'
```

An empty array literal alone doesn't provide enough information for the compiler to infer the intended type of the `Array` instance. When using an empty array literal, specify the type of the variable or constant.  
单独的空数组文字本身并不能为编译器提供足够的信息来推断“Array”实例的预期类型。 使用空数组时，需要指定变量或常量的类型。

```
var emptyArray: [Bool] = []
// 'emptyArray' has type '[Bool]'
```

Because many functions and initializers fully specify the types of their parameters, you can often use an array literal with or without elements as a parameter. For example, the `sum(_:)` function shown here takes an `Int` array as a parameter:  
由于许多函数和初始化函数完全指定了它们的参数类型，所以通常可以使用带或不带元素的数组作为参数。 例如，这里显示的sum（_ :)`函数需要一个`Int`数组作为参数：

```
func sum(values: [Int]) -> Int {
	return values.reduce(0, +)
}

let sumOfFour = sum([5, 10, 15, 20])
// 'sumOfFour' == 50

let sumOfNone = sum([])
// 'sumOfNone' == 0
```

When you call a function that does not fully specify its parameters' types, use the type-cast operator (`as`) to specify the type of an array literal. For example, the `log(name:value:)` function shown here has an unconstrained generic `value` parameter.
当你调用一个没有完全指定它的参数类型的函数时，使用类型转换操作符（`as`）来指定一个字母量数组的类型。 例如，这里显示的`log（name：value：）`函数有一个无约束的泛型`value`参数。

```
func log<T>(name name: String, value: T) {
	print("\(name): \(value)")
}

log(name: "Four integers", value: [5, 10, 15, 20])
// Prints "Four integers: [5, 10, 15, 20]"

log(name: "Zero integers", value: [] as [Int])
// Prints "Zero integers: []"
```
遵守ExpressibleByArrayLiteral 协议(Conforming to ExpressibleByArrayLiteral)
=======================================

Add the capability to be initialized with an array literal to your own custom types by declaring an `init(arrayLiteral:)` initializer. The following example shows the array literal initializer for a hypothetical `OrderedSet` type, which has setlike semantics but maintains the order of its elements.
通过声明一个初始化函数`init（arrayLiteral：）` ，让你的自定义类型可以通过字面量数组来进行初始化.下面的例子展示了给一个假设的具备setlike语义，但是维持元素顺序的假定类型`OrderedSet`添加字面量初始化器
 
```
struct OrderedSet<Element: Hashable>: Collection, SetAlgebra {
	// implementation details
}

extension OrderedSet: ExpressibleByArrayLiteral {
	init(arrayLiteral: Element...) {
		self.init()
		for element in arrayLiteral {
			self.append(element)
		}
	}
}


```
literal: 字面量
surround: 围绕
comma-separated: 逗号分隔符
brackets: 方括号


```
struct ConvertlibleStack<T>: ExpressibleByArrayLiteral, CustomDebugStringConvertible {
    var debugDescription: String {
        get {
            return "stack: \(storage)"
        }
    }
    typealias ArrayLiteralElement = T
    private var storage: [T] = []
    init(arrayLiteral elements: ArrayLiteralElement...) {
        self.storage = elements
    }
    mutating func push(_ item: T) {
        storage.insert(item, at: 0)
    }
    mutating func pop() -> T? {
        guard storage.count > 0 else {
            return nil
        }
        return storage.remove(at: 0)
    }
}

var stack1: ConvertlibleStack = [1,3,4]
var stack2 = ConvertlibleStack(arrayLiteral: 1,3,4)
print(stack1) // print stack: [1, 3, 4]
print(stack2) // print stack: [1, 3, 4]
stack1.pop()
stack1.push(2)
stack2.pop()
stack2.push(2)
```