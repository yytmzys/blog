# [Swift 函数](https://github.com/yytmzys/blog/issues/4)

### 函数
```Swift
        // 函数的定义与调用
        func greet(person: String) -> String {
            let greeting = "Hello, " + person + "!"
            return greeting
        }
        print(greet(person: "Anna"))
        
        // 为了简化这个函数的定义, 可以将问候消息的创建和返回写成一句
        func greetAgain(person: String) -> String {
            return "Hello, again, " + person + "!"
        }
        print(greetAgain(person: "Anna"))
        
        // 函数参数与返回值
        // 无参数函数
        func sayHelloWorld() -> String {
            return "Hello, World!"
        }
        print(sayHelloWorld())
        
        // 多参数函数, 函数可以有多种输入参数, 包含在函数括号中, 以逗号分隔
        func greet2(person: String, alreadyGreeted: Bool) -> String {
            if alreadyGreeted {
                return greetAgain(person: person)
            } else {
                return greet(person: person)
            }
        }
        print(greet2(person: "Tim", alreadyGreeted: true))
        
        // 无返回值函数, 函数可以没有返回值, 不需要返回箭头 -> 和返回类型
        // 严格上来说, 虽然没有返回值被定义, greet2(person:) 函数依然返回了值. 没有定义返回值类型的函数会返回一个特殊的Void值. 它其实是一个空的元祖 touple, 没意义任何元素, 可以写成 ()
        func greet3(person: String) {
            print("Hello, \(person)!")
        }
        greet3(person: "Dave")
        
        // 被调用时, 一个函数的返回值可以被忽略
        func printAndCount(string: String) -> Int {
            print(string)
            return string.count
        }
        
        func pringWithoutCounting(string: String) {
            let _ = printAndCount(string: string)
        }
        printAndCount(string: "Hello, World!")    // 打印, 并返回值
        pringWithoutCounting(string: "Hello, World!")
        
        // 多重返回值函数
        func minMax(array: [Int]) -> (min: Int, max: Int) {
            var currentMin = array[0]
            var currentMax = array[0]
            for value in array[1..<array.count] {
                if value < currentMin {
                    currentMin = value
                } else if value > currentMax {
                    currentMax = value
                }
            }
            
            return(currentMin, currentMax)
        }
        
        let bounds = minMax(array: [8, -9, 2, 109, 2, 76])
        print("min is \(bounds.min) and max is \(bounds.max)")
        
        // 可选元祖返回类型,
        func minMax2(array: [Int]) -> (min: Int, max: Int)? {
            if array .isEmpty { return nil }
            var currentMin = array[0]
            var currentMax = array[0]
            for value in array[1..<array.count] {
                if value < currentMin {
                    currentMin = value
                } else if value > currentMax {
                    currentMax = value
                }
            }
            
            return(currentMin, currentMax)
        }
        let bounds2 = minMax2(array: [8, -9, 2, 109, 2, 76])
        print("min is \(bounds2?.min) and max is \(bounds2?.max)")
        
        // 函数参数标签和参数名称, 你可以在参数名称前指定它的参数标签, 中间以空格分割,
        //    参数标签的使用能够让一个函数在调用时更有表达力, 更类似自然语言, 并且扔保持了函数内部的可读性以及清晰的意图
        func greet4(person: String, from hometown: String) -> String {
            return "Hello \(person)! Glad you could visit from \(hometown)."
        }
        print(greet4(person: "Bill", from: "Cupertino"))
        // Hello Bill! Glad you could visit from Cupertino.
        
        // 忽略函数标签,  如果你不希望为某个参数添加一个标签, 可以使用一个下划线 _ 来代替一个明确的参数标签,
        //           如果一个参数有一个标签, 那么在调用的时候必须用标签来标记这个参数
        func greet5(_ person: String, from hometown:String) -> String {
            return "Hello \(person)! Glad you could visit from \(hometown)."
        }
        print(greet5("Bill", from: "Cupertino"))
        
        // 默认参数值 , 你可以在函数体中通过给参数值来为任意一个参数定义默认值, 当默认值被定以后, 调用这个函数时就可以忽略这个参数
        func greet6(person: String, hometown: String = "earth") -> (String){
            return "Hello \(person)! Glad you could visit from \(hometown)."
        }
        print(greet6(person: "Bill"))
        // Hello Bill! Glad you could visit from earth.
        
        // 可变参数, 可以接受零个或多个值. 函数调用时, 你可以用可变参数来指定函数参数可以被传入不确定数量的输入值. 通过在变量类型名后面加入 ... 的方式来定义可变参数
        // 可变参数的传入值在函数体重变为此类型的一个数组. 例如, 一个叫做numbers 的 Double... 型可变参数, 在函数体内可以当做一个叫numbers 的 [Double]型的数组常量
        func arithmeticMean(_ numbers: Double...) -> Double {
            var total: Double = 0
            for number in numbers {
                total += number
            }
            
            return total / Double( numbers.count )
        }
        
        print(arithmeticMean(1, 2, 3, 4, 5))    // 3.0
        print(arithmeticMean(1, 2, 3, 4, 5, 7)) // 3.66666666666667
        
        // 输入输出参数
        func swapTwoInts(_ a: inout Int, _ b: inout Int ) -> (Int, Int) {
            let temp = a
            a = b
            b = temp
            return (a, b)
        }
        
        var a = 3
        var b = 5
        print(swapTwoInts(&a, &b))  // (5, 3)
        
        // 函数类型. 每个函数都有种特定的函数类型, 函数的类型都由函数的参数类型和返回值类型组成
        func addTwoInts(_ a: Int, _ b: Int) -> Int{
            return a + b
        }
        func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
            return a * b
        }
        // 这两个函数的类型是 (Int, Int) -> Int, 可以解读为" 这个函数类型有两个 Int 型的参数并返回一个 Int 型的值".
        
        // 使用函数类型, 在Swift中, 使用函数类型就像使用其他类型一样.
        var mathFunction: (Int, Int) -> Int = addTwoInts
        // 这段代码可以理解为:  定义一个叫做 mathFunction的常量, 类型是'一个有两个Int型的参数并返回一个Int型的值得函数', 并让这个新变量指向 addTwoInts 函数, addTwoInts 和 mathFunction 有同样的类型, 所以这个赋值 在 Swi 类型检查(type-check) 中是允许的
        print(mathFunction( 2, 3)) // 5
        // 有相同匹配类型的不同函数可以被赋值给同一个变量,
        mathFunction = multiplyTwoInts
        print(mathFunction( 2, 3)) // 6
        
        
        // 函数类型作为参数类型,  你可以用 (Int, Int) -> Int 这样的函数类型最为另一个函数的参数类型. 这样可以将函数的一部分实现留给函数的调用者来提供
        func printMathResult(_ mathFunction: (Int, Int) -> Int, a: Int, _ b: Int) {
            print("Result: \(mathFunction(a, b))")
        }
        printMathResult(addTwoInts, a: 2, 3) // Result: 8
        
        // 函数类型作为返回类型, 你可以用函数类型作为另一个函数的返回类型, 你需要做的就是在返回箭头 -> 后写一个完整的函数类型
        func stepForward(_ input: Int) -> Int{
            return input + 1
        }
        
        func stepBackward(_ input: Int) -> Int {
            return input - 1
        }
        // 如下名为chooseStepFunction的函数, 它的返回值类型是 (Int) -> int 类型的函数
        func chooseStepFunction(backward: Bool) -> (Int) -> Int {
            return backward ? stepForward : stepBackward
        }
        
        var currentValue = 3
        var moveNearerToZero = chooseStepFunction(backward: currentValue > 5)
        // moveNearerToZero 现在指向 stepBackward
        while currentValue != 0 {
            print("\(currentValue)")
            currentValue = moveNearerToZero(currentValue)
        }
        print("zero!")
        
        // 嵌套函数, 可以把函数定义在别的函数体中.    默认情况下, 嵌套函数对外界是不可见的, 但是可以被它们的外围函数(enclosing function) 调用.    一个外围函数也可以返回它的某一个嵌套函数, 使得这个函数可以在其他领域中被使用
        func chooseStepFunction2(backward: Bool) -> (Int) -> Int {
            func stepForward(input: Int)  -> Int { return input + 1}
            func stepBackward(input: Int) -> Int { return input - 1}
            return backward ? stepForward : stepBackward
        }
        
        currentValue = -4
        moveNearerToZero = chooseStepFunction2(backward: currentValue < 0)
        // moveNearerToZero 指向 stepForward
        while currentValue != 0 {
            print("\(currentValue)")
            currentValue = moveNearerToZero(currentValue)
        }
        print("zero!")
```

### 闭包, 在 Swift 中，Block 被称为闭包（Closures），示例代码如下：

1  非逃逸闭包（Non-Escaping Closure）：
```Swift
func performOperation(completion: () -> Void) {
    print("Performing operation...")
    completion()
}

performOperation {
    print("Operation completed.")
}
```

2 逃逸闭包（Escaping Closure）：
```Swift
var completionHandlers: [() -> Void] = []

func appendCompletionHandler(handler: @escaping () -> Void) {
    completionHandlers.append(handler)
}

func performOperation(completion: () -> Void) {
    print("Performing operation...")
    completionHandlers.append(completion)
}

performOperation {
    print("Operation completed.")
}

completionHandlers.forEach { $0() }
```

3 自动闭包（Autoclosure）：
```Swift
func printAndRemove(element: @autoclosure () -> Int) {
    print("Element:", element())
}

printAndRemove(element: 10)
```
这些示例展示了不同类型的闭包在 Objective-C 和 Swift 中的用法。全局块类似于 Swift 中的全局函数，而栈块和堆块用于捕获局部变量并在不同的作用域中使用。在 Swift 中，根据闭包是否允许逃逸，以及是否自动封装表达式，有更多的灵活性和功能。
