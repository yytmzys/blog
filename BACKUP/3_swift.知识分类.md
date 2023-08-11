# [swift 知识分类](https://github.com/yytmzys/blog/issues/3)

swift 知识分类
----
### 1 语法和基础：
变量、常量和数据类型 https://github.com/yytmzys/blog/issues/3#issuecomment-1674663958
运算符 https://github.com/yytmzys/blog/issues/3#issuecomment-1674691646
表达式 https://github.com/yytmzys/blog/issues/3#issuecomment-1674831952
控制流程：if、switch、循环 https://github.com/yytmzys/blog/issues/3#issuecomment-1674878083
函数和闭包 https://github.com/yytmzys/blog/issues/3#issuecomment-1674887137
类、结构体和枚举
属性和方法
协议和扩展

### 1 可选项和错误处理：
可选项（Optionals）
强制解包和可选项绑定
错误处理（Error Handling）

### 1 集合类型：
数组（Array）
字典（Dictionary）
集合（Set）

### 1 函数式编程：
高阶函数
map、filter、reduce
尾随闭包

### 1 并发和多线程：
Grand Central Dispatch（GCD）
Operation 和 OperationQueue
异步编程

### 1 内存管理：
引用类型和值类型
自动引用计数（ARC）
内存管理模式面向对象编程：
继承和多态
初始化器和反初始化器
访问控制

### 1 模式设计：
单例模式
工厂模式
观察者模式等

### 1 界面开发：
SwiftUI 和 UIKit
视图控制器和视图
自定义视图和界面布局

### 1 网络和数据：
网络请求和 URLSession
JSON 解析和编码
数据持久化：Core Data、UserDefaults

### 1 测试和调试：
单元测试和 UI 测试
断言和调试技巧
Instruments 工具

### 1 设计模式：
MVC、MVVM、VIPER
依赖注入、装饰器等

### 1 工具和开发环境：
Xcode 使用技巧
Swift Package Manager
调试器和性能优化

### 1 Swift 版本更新：
Swift 语言的版本更新和变化




---

变量、常量和数据类型
------
1. 声明常量和变量
```Swift
let maxNum = 1000  // 声明常量
//maxNum = 1001        // 不可变

var index = 2               // 声明变量
index = 3                       // 可变

var x = 1, y = 2, z = 3  // 同时声明多个值, 逗号分隔
// Type inference 自动判断值的类型, 固定为初值类型
//x = "hello"                    
var s = "hello"

let website: String = "www.imooc.com"  // 显式声明类型
var a, b, c: Double       // 同时显式声明多个变量

```

2. 整形
```Swift
var imInt: Int = 80
Int.max
Int.min

//imInt = 99999999999999999999 编译时报错

var imUIntf: UInt = 80
UInt.max  // 无符号整形, 不能存储负数
UInt.min  // 最小是0,


Int8.max
Int8.min
UInt8.max
UInt8.min

Int16.max
Int16.min
UInt16.max
UInt16.min

Int32.max
Int32.min
UInt32.max
UInt32.min

Int64.max
Int64.min
UInt64.max
UInt64.min

let decimalInt: Int = 17       // 十进制
let binaryInt: Int  = 0b10001  // 二进制
let octalInt: Int   = 0o21     // 八进制
let hexInt: Int     = 0x11     // 十六进制

let bigNum1 = 1_000_000         // 下划线分隔大数字, 自由分隔位置
let bigNmu2 = 1_00_0000         //
```

```Swift
/*  浮点数,小数 */
let imFlouat: Float  = 3.1415926 // 3.141593
let imDouble: Double = 3.1415926 // 3.1415926

let x = 3.1415926 // 自动判定类型为 Double
var a = 1.25e10   // 1.25 乘以 10 个 10
var b = 1.25e-8   // 1.25 除以 8 个 10
var c = 1_000_000.000_000_1 // 下划线分隔, 增强可读性
```

```Swift
/* 类型转换 */
let x: UInt16 = 100
let y: UInt8  = 20
//x + y  不支持隐式类型转换, 只能显式转换, 要求严格, 规避错误
let m = x + UInt16(y)

let w:Float = 3         // 3为浮点数, 但未自动转换类型
//let v:Int   = 3.0     // 3.0判断为浮点数, 不能赋值给Int值 v
let v:Int   = Int(3.0)
```

```Swift
/* 布尔类型 简单if */
let imTrue:Bool  = true
let imFalse      = false
let a = 1

if imTrue {
    print("I'm true")
}else{
    print("I'm false")
}

if 1{   // 报错, 1 不在作为true被执行, 对比其他语言1=true, false=0, 'Int' is not convertible to 'Bool'
    print("执行")
}
```

```Swift
/* Touple 元祖 */
// 将任意个不同的值集合成一个数据, 不同的值可以是不同类型, 小括号中书写数据
var point = ( 5 , 2)
var ( x , y) = point
x        // 访问元祖的值, 方式1
point.0  // 访问元祖的值  方式2

var httpResponse:(Int , String) = (404 , "Not Found!")  //  显式声明元祖数据

let point2:(x: Int, y:Int ) = (3 , 2) // 声明元祖 指定名称1
let point3 = (x: 3 , y: 2)            // 声明元祖 指定名称2

let (_ , httpResult) = httpResponse   // 使用下划线忽略元祖其他值
httpResult
```

```Swift
/* 命名方式 = 首字母大写, 对齐方式 = 名称+类型+值 各自对齐  */
let imInt:    Int    = 3
let imFloat:  Float  = 3.14
let imDouble: Double = 3.1415926
let imBool:   Bool   = true
let imString: String = "Hello, Swift:)"

var 名字 = "wangpl"        // 常量, 变量名字非常开放, uicode编码都可以
print("我的名字是" + 名字)

var 😀 = "smile"
print(名字 + 😀)

/* print输出方式 */
let x = 1, y = 2, z = 3 , b = true
print(x,y,z, b)                       // 输出不同值时, 自带空格分隔符和尾换行
print(x , y , z , separator:",")      // separator表示重置/替换 输出不同值时替换的分隔符
print(x , y , z , terminator:":)")    // separator表示重置/替换 尾换行
print("\(y) * \(z) = \(y*z)")         // 双引号里插入 \() ,表示将相应的值 放在该位置
```

```Swift
/*  可选类型  */
let possilebNumber  = "123"
let convertedNumber = Int(possilebNumber)  // 尝试将一个 String 转换成 Int, 此时 convertedNumber 的类型是可选类型 Int?

// nil    Swift 中是一个确定的值，用来表示值缺失, 任何类型的可选状态都可以被设置为 nil，不只是对象类型。
//        OC    中, nil是一个空指针,  指向不存在对象的指针
var serverResponseCocde: Int? = 404
serverResponseCocde = nil
var a = 10  // 声明一个Int a, 后赋值 nil , 报错 Nil cannot be assigned to type 'Int'
//a = nil
var b: String? // 此时 b 的值自动设为 nil

// if 语句及强制解析
if convertedNumber != nil { // 如果可选类型有值, 它将不等于 nil
    print("convertedNumber有值") // 输出 有值
}
// 强制解析: 当确定可选类型有值时, 可以在可选类型名字后面加一个感叹号 ! 来获取值, 这个惊叹号表示"我知道这个可选有值, 请使用它"
if convertedNumber != nil {
    print("\(convertedNumber!)") // 输出 123
}

// 可选绑定 (optional binding),  用来判断可选类型是否包含值, 如果包含就把值赋给一个临时常量/变量,
// 可选绑定可以用在if 和 while语句中, 这条语句不仅可以用来判断可选类型中是否有值, 同时可以将可选类型中的值赋给一个常量或者变量
if let actualNumber = Int(possilebNumber) {
    actualNumber // 值为 123
}else{
    print("无")
}
// 你可以包含多个可选绑定或多个布尔条件在一个 if 语句中, 只要使用逗号分开就行. 只要有任意一个可选绑定的值为 nil, 或者任意一个布尔条件为false, 则真个 if 条件判断为false, 这时你就需要使用嵌套if条件语句来处理, 如下所示
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100{
    print("\(firstNumber) < \(secondNumber) < 100")  // 输出
}

if let firstNumber = Int("4") {
    if let secondNumber = Int("42") {
        if firstNumber < secondNumber && secondNumber < 100 {
            print("\(firstNumber) < \(secondNumber) < 100")
        }
    }
}
// 输出 "4 < 42 < 100"


// 隐式解析 , 自动解析可选类型
let str1: String? = "An optional string"
let str2: String  = str1! // 需要感叹号来获取值

let str3: String! = "An implicitly unwrapped optional string"
let str4: String  = str3  // 不需要感叹号
// 如果一个变量可能变成 nil 的话, 请不要使用隐式解析可选类型


```

---

Swift 运算符
----
运算符是一个符号，用于告诉编译器执行一个数学或逻辑运算。
Swift 提供了以下几种运算符：
1 算术运算符
2 比较运算符
3 逻辑运算符
4 位运算符
5 赋值运算符
6 区间运算符
7 其他运算符


#### 算术运算符
以下表格列出了 Swift 语言支持的算术运算符，其中变量 A 为 10，变量 B 为 20：

| 运算符 |	描述	 | 实例 |
|---|---|---|
| +   |	加号		|	A + B 结果为 30
| −   |	减号		| 	A − B 结果为 -10
| *   |		乘号	 	| 	A * B 结果为 200
| /   |		除号		| 	B / A 结果为 2
| % |		求余		| 	B % A 结果为 0

```Swift

var A = 10
var B = 20

print("A + B 结果为：\(A + B)")
print("A - B 结果为：\(A - B)")
print("A * B 结果为：\(A * B)")
print("B / A 结果为：\(B / A)")
A += 1   // 类似 A++
print("A += 1 后 A 的值为 \(A)")
B -= 1   // 类似 B--
print("B -= 1 后 B 的值为 \(B)")

```
<img width="1019" alt="1" src="https://github.com/yytmzys/blog/assets/45475313/41dd9030-daa5-40e2-bc9f-e7e7d5101c92">


#### 比较运算符
以下表格列出了 Swift 语言支持的比较运算符，其中变量 A 为 10，变量 B 为 20：
| 运算符 |	描述	| 实例 |
|---|---|---|
| ==	| 等于	 | (A == B) 为 false。
| !=	| 不等于	 | (A != B) 为 true。
| >	| 大于	 | (A > B) 为 false。
| <	| 小于	 | (A < B) 为 true。
| >=	| 大于等于	 | (A >= B) 为 false。
| <=	| 小于等于	 | (A <= B) 为 true。

<img width="996" alt="2" src="https://github.com/yytmzys/blog/assets/45475313/7439f3de-f237-4f93-99fb-a5444c4c21d2">

#### 逻辑运算符
以下表格列出了 Swift 语言支持的逻辑运算符，其中变量 A 为 true，变量 B 为 false：
| 运算符 |	描述	| 实例 |
|---|---|---|
|  && |  逻辑与 | 如果运算符两侧都为 TRUE 则为 TRUE。|	(A && B) 为 false。
|  \|\|	|  逻辑或 |  如果运算符两侧至少有一个为 TRUE 则为 TRUE。|	(A \|\| B) 为  true。
|  !	|  逻辑非 | 布尔值取反，使得true变false，false变true。|	!(A && B) 为 true。

<img width="967" alt="3" src="https://github.com/yytmzys/blog/assets/45475313/34d94922-aa91-4cc3-915e-d4194995d0dd">


#### 位运算符
位运算符用来对二进制位进行操作，~,&,|,^分别为取反，按位与与，按位与或，按位与异或运算，如下表实例：
| 运算符 |	描述	| 实例 |
|---|---|---|
| p | 	q | 	p & q	p | q	p ^ q
| 0 | 	0 | 	0	0	0
| 0 | 	1 | 	0	1	1
| 1 | 	1 | 	1	1	0
| 1 | 	0 | 	0	1	1

#### 赋值运算
下表列出了 Swift 语言的基本赋值运算
| 运算符 |	描述	| 实例 |
|---|---|---|
|    =     |   简单的赋值运算，指定右边操作数赋值给左边的操作数。 | C = A + B 将 A + B 的运算结果赋值给 C
|    +=   |   相加后再赋值，将左右两边的操作数相加后再赋值给左边的操作数。 | C += A 相当于 C = C + A
|    -=   |   	相减后再赋值，将左右两边的操作数相减后再赋值给左边的操作数。 | C -= A 相当于 C = C - A
|    *=   |   	相乘后再赋值，将左右两边的操作数相乘后再赋值给左边的操作数。 | C *= A 相当于 C = C * A
|    /=   |   	相除后再赋值，将左右两边的操作数相除后再赋值给左边的操作数。 | C /= A 相当于 C = C / A
|    %=   |   	求余后再赋值，将左右两边的操作数求余后再赋值给左边的操作数。 | 	C %= A is equivalent to C = C % A
|    <<=   |   	按位左移后再赋值	 | C <<= 2 相当于 C = C << 2
|    >>=   |   	按位右移后再赋值	 | C >>= 2 相当于 C = C >> 2
|    &=    |   	按位与运算后赋值	 | C &= 2 相当于 C = C & 2
|    ^=     |   按位异或运算符后再赋值  | C ^= 2 相当于 C = C ^ 2
|    \|=     |   按位或运算后再赋值	 | C \|= 2 相当于 C = C \| 2

```
var A = 10
var B = 20
var C = 100

C = A + B
print("C 结果为：\(C)")

C += A
print("C 结果为：\(C)")

C -= A
print("C 结果为：\(C)")

C *= A
print("C 结果为：\(C)")

C /= A
print("C 结果为：\(C)")

//以下测试已注释，可去掉注释测试每个实例
C %= A
print("C 结果为：\(C)")


C <<= A
print("C 结果为：\(C)")

C >>= A
print("C 结果为：\(C)")

C &= A
print("C 结果为：\(C)")

C ^= A
print("C 结果为：\(C)")

C |= A
print("C 结果为：\(C)")
```

<img width="922" alt="5" src="https://github.com/yytmzys/blog/assets/45475313/75285aac-53ba-4a5d-a304-a07446f321e0">


#### 区间运算符, 用于 for 循环
Swift 提供了两个区间的运算符。

| 运算符|	描述	| 实例 |
|---|---|---|
| 闭区间运算符 | （a...b）定义一个包含从a到b(包括a和b)的所有值的区间，b必须大于等于a。 ‌ 闭区间运算符在迭代一个区间的所有值时是非常有用的，如在for-in循环中：| 1...5 区间值为 1, 2, 3, 4 和 5
| 半开区间运算符 | （a..<b）定义一个从a到b但不包括b的区间。 之所以称为半开区间，是因为该区间包含第一个值而不包括最后的值。| 1..< 5 区间值为 1, 2, 3, 和 4
```Swift
print("闭区间运算符:")
for index in 1...5 {
    print("\(index) * 5 = \(index * 5)")
}

print("半开区间运算符:")
for index in 1..<5 {
    print("\(index) * 5 = \(index * 5)")
}
```
<img width="905" alt="6" src="https://github.com/yytmzys/blog/assets/45475313/4efc69f5-e123-4637-bba8-3b646fc5061f">


#### 其他运算符
Swift 提供了其他类型的的运算符，如一元、二元和三元运算符。

一元运算符对单一操作对象操作（如-a）。一元运算符分前置运算符和后置运算符，前置运算符需紧跟在操作对象之前（如!b），后置运算符需紧跟在操作对象之后（例如c!）。备注：在Java／C没有类似c！的语法， 在Swift中用在Optional类型取值。
二元运算符操作两个操作对象（如2 + 3），是中置的，因为它们出现在两个操作对象之间。
三元运算符操作三个操作对象，和 C 语言一样，Swift 只有一个三元运算符，就是三目运算符（a ? b : c）。

| 运算符|	描述	| 实例 |
|---|---|---|
| 一元减 | 数字前添加 - 号前缀   |   -3 或 -4
| 一元加 | 数字前添加 + 号前缀   | +6 结果为 6
| 三元运算符 | condition ? X : Y   | 如果 condition 为 true ，值为 X ，否则为 Y
```
var A = 1
var B = 2
var C = true
var D = false
print("-A 的值为：\(-A)")
print("A + B 的值为：\(A + B)")
print("三元运算：\(C ? A : B )")
print("三元运算：\(D ? A : B )")
```
<img width="915" alt="8" src="https://github.com/yytmzys/blog/assets/45475313/7fd83f21-2a87-4acc-a865-c2a6302b9815">


#### 运算符优先级
在一个表达式中可能包含多个有不同运算符连接起来的、具有不同数据类型的数据对象；由于表达式有多种运算，不同的运算顺序可能得出不同结果甚至出现错误运算错误，因为当表达式中含多种运算时，必须按一定顺序进行结合，才能保证运算的合理性和结果的正确性、唯一性。

优先级从上到下依次递减，最上面具有最高的优先级，逗号操作符具有最低的优先级。

相同优先级中，按结合顺序计算。大多数运算是从左至右计算，只有三个优先级是从右至左结合的，它们是单目运算符、条件运算符、赋值运算符。

基本的优先级需要记住：

指针最优，单目运算优于双目运算。如正负号。
先乘除（模），后加减。
先算术运算，后移位运算，最后位运算。请特别注意：1 << 3 + 2 & 7 等价于 (1 << (3 + 2))&7
逻辑运算最后计算
Swift 运算符优先级 (从高到低)：

| 运算符|	描述	| 
|---|---|
| 位运算符	| >> &<< &>> >>     |
| 乘法运算符	| &* % & * /             |
| 加法运算符	| \| &+ &- + -  ^        |
| 区间运算符	| ..< ...                     |
| 类型转换运算符	| is as             |
| nil 的聚合运算	| ??                 |
| 比较运算符	| != > < >= <= === ==       |
| 逻辑与运算符	| &&                |
| 逻辑或运算符	| \|\|                 |
| 波浪箭头	| ~>                        |
| 三元运算符	| ?:                         |
| 箭头函数	| ( )                         |
| 赋值运算符	| \|= %= /= &<<= &>>= &= *= >>= <<= ^= += -=        |

---

Swift 表达式
-----
1   前缀表达式-Prefix Expressions
2   二元表达式（Binary Expressions）
3   赋值表达式（Assignment Operator）
4   三元条件运算符（Ternary Conditional Operator）
5   类型转换运算符（Type-Casting Operators）
6   主要表达式（Primary Expressions）
7   后缀表达式（Postfix Expressions）

表达式可以返回一个值，以及运行某些逻辑（causes a side effect）。
#### Swift 中存在四种表达式：
1 前缀（prefix）表达式 
2 二元（binary）表达式
3 主要（primary）表达式
4 后缀（postfix）表达式

前缀表达式和二元表达式就是对某些表达式使用各种运算符（operators）。 
主要表达式是最短小的表达式，它提供了获取（变量的）值的一种途径。 
后缀表达式则允许你建立复杂的表达式，例如配合函数调用和成员访问。 

#### 前缀表达式（Prefix Expressions）
前缀表达式由 前缀符号和表达式组成。（这个前缀符号只能接收一个参数）

Swift 标准库支持如下的前缀操作符：
++ 自增1 （increment）
-- 自减1 （decrement）
! 逻辑否 （Logical NOT ）
~ 按位否 （Bitwise NOT ）
\+ 加（Unary plus）
\- 减（Unary minus）

#### 二元表达式（Binary Expressions）
二元表达式由 "左边参数" + "二元运算符" + "右边参数" 组成, 它有如下的形式：

left-hand argument operator right-hand argument

Swift 标准库提供了如下的二元运算符：
##### 求幂相关（无结合，优先级160）
<< 按位左移（Bitwise left shift）
按位右移（Bitwise right shift）

##### 乘除法相关（左结合，优先级150）
\* 乘
/ 除
% 求余
&* 乘法，忽略溢出（ Multiply, ignoring overflow）
&/ 除法，忽略溢出（Divide, ignoring overflow）
&% 求余, 忽略溢出（ Remainder, ignoring overflow）
& 位与（ Bitwise AND）

##### 加减法相关（左结合, 优先级140）
\+ 加
\- 减
&+ Add with overflow
&- Subtract with overflow
| 按位或（Bitwise OR ）
^ 按位异或（Bitwise XOR）

##### Range （无结合,优先级 135）
..< 半闭值域 Half-closed range
... 全闭值域 Closed range

##### 类型转换 （无结合,优先级 132）
is 类型检查（ type check）
as 类型转换（ type cast）

##### Comparative （无结合,优先级 130）
< 小于
<= 小于等于
\> 大于
\>= 大于等于
== 等于
!= 不等
=== 恒等于
!== 不恒等
~= 模式匹配（ Pattern match）
##### 合取（ Conjunctive） （左结合,优先级 120）
&& 逻辑与（Logical AND）
##### 析取（Disjunctive） （左结合,优先级 110）
|| 逻辑或（ Logical OR）
##### 三元条件（Ternary Conditional ）（右结合,优先级 100）
?: 三元条件 Ternary conditional
##### 赋值 （Assignment） （右结合, 优先级 90）
= 赋值（Assign）
*= Multiply and assign
/= Divide and assign
%= Remainder and assign
+= Add and assign
-= Subtract and assign
<<= Left bit shift and assign
>>= Right bit shift and assign
&= Bitwise AND and assign
^= Bitwise XOR and assign
|= Bitwise OR and assign
&&= Logical AND and assign
||= Logical OR and assign


https://doc.yonyoucloud.com/doc/the-swift-programming-language-in-chinese/chapter3/04_Expressions.html

---

控制流程：if、switch、循环
----
### Swift 条件语句
条件语句通过设定的一个或多个条件来执行程序，在条件为真时执行指定的语句，在条件为 false 时执行另外指定的语句。

Swift 提供了以下几种类型的条件语句：

| 语句 | 描述 |
|---|---|
|  if 语句 | **if 语句** 由一个布尔表达式和一个或多个执行语句组成。 |
| if...else | **if 语句** 后可以有可选的 **else 语句**, **else 语句**在布尔表达式为 false 时执行。 |
| if...else if...else  | **if** 后可以有可选的 **else if...else** 语句, **else if...else** 语句常用于多个条件判断。 |
| 内嵌 if 语句 | 你可以在 **if** 或 **else if** 中内嵌 **if** 或 **else if** 语句。 |
| switch 语句 | switch 语句允许测试一个变量等于多个值时的情况。 |

### ? :  三目运算符

我们已经在前面的章节中讲解了 **条件运算符 ? :**，可以用来替代 **if...else** 语句。它的一般形式如下：
Exp1 ? Exp2 : Exp3;
其中，Exp1、Exp2 和 Exp3 是表达式。请注意，冒号的使用和位置。

? 表达式的值是由 Exp1 决定的。如果 Exp1 为真，则计算 Exp2 的值，结果即为整个 ? 表达式的值。如果 Exp1 为假，则计算 Exp3 的值，结果即为整个 ? 表达式的值。


## Swift 循环
有的时候，我们可能需要多次执行同一块代码。一般情况下，语句是按顺序执行的：函数中的第一个语句先执行，接着是第二个语句，依此类推。

编程语言提供了更为复杂执行路径的多种控制结构。

循环语句允许我们多次执行一个语句或语句组，下面是大多数编程语言中循环语句的流程图：

### 循环类型
| 循环类型 | 描述 |
| ---|---|
| for-in  | 遍历一个集合里面的所有元素，例如由数字表示的区间、数组中的元素、字符串中的字符。 |
| while 循环 | 运行一系列语句，如果条件为true，会重复运行，直到条件变为false。 |
| repeat...while 循环 | 类似 while 语句区别在于判断循环条件之前，先执行一次循环的代码块。 |

#### 1 for-in 循环用于遍历一个集合里面的所有元素，例如由数字表示的区间、数组中的元素、字符串中的字符。
```Swift
for index in 1...5 {
    print("\(index) 乘于 5 为：\(index * 5)")
}
```
<img width="847" alt="1" src="https://github.com/yytmzys/blog/assets/45475313/8b96e7e0-fd56-4a95-af12-5d2889901a8b">



#### 2 while循环从计算单一条件开始。如果条件为true，会重复运行一系列语句，直到条件变为false。
```Swift
var index = 10

while index < 20
{
   print( "index 的值为 \(index)")
   index = index + 1
}
```
<img width="852" alt="2" src="https://github.com/yytmzys/blog/assets/45475313/3d2a0c4a-80fd-497c-bcca-13e81d348146">


#### 3 repeat...while 循环不像 for 和 while 循环在循环体开始执行前先判断条件语句，而是在循环执行结束时判断条件是否符合。
```Swift
var index = 15

repeat{
    print( "index 的值为 \(index)")
    index = index + 1
}while index < 20
```
<img width="861" alt="3" src="https://github.com/yytmzys/blog/assets/45475313/22173699-cddd-4a85-b334-286a83d9c810">



### 循环控制语句
循环控制语句改变你代码的执行顺序，通过它你可以实现代码的跳转。Swift 以下几种循环控制语句：

| 控制语句 | 描述 |
| -|-|
| continue 语句 | 告诉一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。 |
| break 语句 | 中断当前循环。 |
| fallthrough 语句 | 如果在一个case执行完后，继续执行下面的case，需要使用fallthrough(贯穿)关键字。 |

#### 1 continue语句告诉一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。

对于 for 循环，continue 语句执行后自增语句仍然会执行。对于 while 和 do...while 循环，continue 语句重新执行条件判断语句。
```Swift
 var index = 10

repeat{
   index = index + 1
  
   if( index == 15 ){ // index 等于 15 时跳过
      continue
   }
   print( "index 的值为 \(index)") // index 的值 最终为 20
}while index < 20

```
<img width="865" alt="4" src="https://github.com/yytmzys/blog/assets/45475313/430f0ce5-909c-40cf-a244-526c18d4d8ad">



#### 2 break语句会立刻结束整个控制流的执行。

如果您使用的是嵌套循环（即一个循环内嵌套另一个循环），break 语句会停止执行最内层的循环，然后开始执行该块之后的下一行代码。
```Swift
var index = 10

repeat{
    index = index + 1
    
    if( index == 15 ){  // index 等于 15 时终止循环
        break
    }
    print( "index 的值为 \(index)")
}while index < 20
```
<img width="864" alt="5" src="https://github.com/yytmzys/blog/assets/45475313/debdc150-6004-4441-8e40-ac3a1b72cf4e">


#### 3 fallthrough 语句让 case 之后的语句会按顺序继续运行，且不论条件是否满足都会执行。

Swift 中的 switch 不会从上一个 case 分支落入到下一个 case 分支中。只要第一个匹配到的 case 分支完成了它需要执行的语句，整个switch代码块完成了它的执行。
```Swift

var index = 10

switch index {
   case 100  :
      print( "index 的值为 100")
      fallthrough
   case 10,15  :
      print( "index 的值为 10 或 15")
      fallthrough
   case 5  :
      print( "index 的值为 5")
   default :
      print( "默认 case")
}
```
<img width="947" alt="6" src="https://github.com/yytmzys/blog/assets/45475313/7cd1581e-d85f-4cf8-83ac-62ce6a001f82">



---

函数和闭包
-----
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
