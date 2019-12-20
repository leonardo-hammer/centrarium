---
layout: post
title: Swift Tour
comments: true
---

### “Use let to make a constant and var to make a variable”

let 定义常量

var 定义变量

### “simpler way to include values in strings: Write the value in parentheses, and write a backslash (\) before the parentheses.”

连接字符串的简易方法: `"\()"`

{% highlight swift %}
let apples = 3
let appleSummary = "I have \(apples) apples.
{% endhighlight %}

### “Use three double quotation marks (""") for strings that take up multiple lines”

在代码中，使用三个双引号 `"""` 来赋值多行文本

{% highlight swift %}
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit.
"""
{% endhighlight %}

### “Create arrays and dictionaries using brackets ([])”

创建数组的方式 `= []`

{% highlight swift %}
var shoppingList = ["catfish", "water", "tulips"]
{% endhighlight %}

### “Use if and switch to make conditionals, and use for-in, while, and repeat-while to make loops. ”

`if` 的使用基本和其他语言一样，多了一个 optional 的便捷判断，`if let variable { }`，当变量 variable 不为 nil 的时候，if 语句判断为 true

`switch` 在匹配到了 case 之后，会自动 break，所以不需要写 break，但是需要穷举所有 switch 的可能性，否则报错。最简单的方式是末尾写一个 default

`for-in` 是很便捷的循环，循环 Dictionary 或者 Array，以下给出循环示例

{% highlight swift %}
// Dictionary 中的 value 是一个 Array
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]

for (kind, numbers) in interestingNumbers {
    for number in numbers {
        print(number)
    }
}
{% endhighlight %}

`while` 循环是先判断条件，为 true 才执行

`repeat-while` 循环是先执行一次，然后再判断条件，条件为 true 才继续执行第二次。类似 do-while

### “Another way to handle optional values is to provide a default value using the ?? operator”

处理 optional 类型的变量有一种便捷的方式，就是给他指定一个默认值，当变量为 nil 的时候，会使用这个默认值。

使用两个问号 `??` 跟在 optional 变量的后面

{% highlight swift %}
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickName ?? fullName)
{% endhighlight %}

### “Use -> to separate the parameter names and types from the function’s return type.”

使用 `->` 来指明一个 function 的返回值类型

{% highlight swift %}
func greet() -> String {
    return "Greeting."
}
{% endhighlight %}

### “Write a custom argument label before the parameter name, or write _ to use no argument label”

如果不需要 function 中的参数标签，可以加上下划线占位符 `_` 在变量名前面。之后调用方法的时候，就不用写参数标签了

{% highlight swift %}
// 有参数标签
func greet(name: String) -> String {
    return "Greeting, \(name)"
}
greet(name: "Tue")

// 省略参数标签
func greetTo(_ name: String) -> String {
    return "Greeting, \(name)"
}
greetTo("Tue")
{% endhighlight %}

### “a function can return another function as its value.”

function 可以用另外一个 function 来作为返回值

{% highlight swift %}
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
{% endhighlight %}

### “A function can take another function as one of its arguments.”

function 可以成为另外一个 function 的参数

{% highlight swift %}
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
{% endhighlight %}

作为参数的时候，是可以有省略写法的

{% highlight swift %}
var numbers = [20, 19, 7, 12]

// 普通写法
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})

// 简略写法
var numbersMapped = numbers.map {number in number * 10}

// 极简写法，其中 $0 表示遍历到的当前数组的元素
numbersMapped = numbers.map { $0 * 10 }
{% endhighlight %}

### “Create an instance of a class by putting parentheses after the class name.”

创建一个 class 的实例对象，使用 class 名称和括号就可以

{% highlight swift %}
var shape = Shape()
{% endhighlight %}

class 中的初始化和销毁的生命周期方法：`init` 和 `deinit`

### “properties can have a getter and a setter.”

可以自定 class 中 property 的 getter 和 setter 方法

{% highlight swift %}
class EquilateralTriangle: NamedShape {
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
}
{% endhighlight %}

在 setter 方法中，`newValue` 是一个特殊内部值，表示修改 property 传入的新值

### “run before and after setting a new value, use willSet and didSet. ”

在 set 执行前和执行后，可以自定两个方法 `willSet` 和 `didSet`

{% highlight swift %}
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
}
{% endhighlight %}

### “Use enum to create an enumeration.”

枚举的定义使用关键字 `enum`

### “Use struct to create a structure.”

结构体的定义使用关键字 `structure`

### “structures are always copied when they are passed around in your code, but classes are passed by reference.”

structure 和 class 最大的区别是，structure 总是一个值的传递，而 class 是对象引用的传递

传递之后，对于 structure 的任何修改，不会影响到之前的那个 structure；而 class 正好相反，传递前后的对象，都是同一个，任何地方做出了修改，都会一同修改

### “Use protocol to declare a protocol.”

协议的创建使用关键字 `protocol`

### “Use extension to add functionality to an existing type”

扩展的关键字 `extension`

使用扩展来实现一个 protocol 是很常用的方式

### “You represent errors using any type that adopts the Error protocol”

创建一个 Error，实现 Error 协议即可

{% highlight swift %}
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
{% endhighlight %}

### “Use throw to throw an error and throws to mark a function that can throw an error.”

在 function 后面加上 `throws` 表明这个函数有可能会抛出错误

在代码中，使用 `throw` 关键字来抛出一个特定的错误

{% highlight swift %}
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
{% endhighlight %}

处理 Error 有几种途径：

**使用 `do-catch` 代码块来处理**

{% highlight swift %}
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
{% endhighlight %}

在 do 的代码块中，使用 `try` 开头来调用可能 throws 的 function

并在 catch 中获取到隐藏的对象 error，进行处理，这个 error 的名称是默认的，也可以自定义

**使用 `try?` 来直接调用**

这样写起来比较简便，当出现错误的时候，function 会返回 nil，适用于不需要处理 Error 的场景

{% highlight swift %}
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
{% endhighlight %}

### “You can use defer to write setup and cleanup code next to each other, even though they need to be executed at different times.”

使用 `defer` 关键字创建一个代码块，里面的代码会在整个 function 执行完成之后才执行

适用于有多种 if else 都可能 return 的场景，来做一些释放的操作

### “Write a name inside angle brackets to make a generic function or type.”

使用尖括号来创建一个泛型

{% highlight swift %}
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
{% endhighlight %}

代码中的 `Item` 不是任何 class 或者 structure，就是一个符号，它确保了整个 function 中，所有的 Item 都是同一个类型。

也就是说，可以传入任何类型，会自动匹配
