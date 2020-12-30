## 标准输入输出

### 格式化占位符

- Printf("%T\n", var) 可以打印某个变量的类型：int / int8 / int16 ...
- Printf("%v\n", var) 可以打印某个变量的值，可以是任意变量
- Printf("%v\n", var) 类似%v，但输出结构体时会添加字段名
- Printf("%#v\n", var) 可以帮你在打印string的时候加引号，会显示更多的细节
- Printf("%d\n", var) decimal
- Printf("%b\n", var) binary
- Printf("%t\n", var) boolean
- Printf("%o\n", var) octal
- Printf("%x\n", var) hex，使用a~f
- Printf("%X\n", var) hex，使用A~F
- Printf("%s\n", var) 直接输出字符串或者[]byte
- Printf("%U\n", var) unicode
- Printf("%p\n", &var) physical address, in hex form
- \是转义字符
- %%就是一个'%'，为了告诉这个%不是一个占位符

反引号``里的东西会保持原样输出，包括前面缩紧的空格，里面的内容无需转义字符

### 输入

#### Scan

获取用户输入

```go
func Scan(a ...interface{}) (n int, err error) // ...表示可以穿多个值
// Scan从标准输入扫描文本，读取由空白符分隔的值传递给本函数的参数中，换行符视为空白符

func main() {
    var s string
    fmt.Scan(&s)
    fmt.Println("用户输入的内容是："，s)
}
```

#### Scanf

```go
func main() {
    var(
		name string
    	age int
    	class string
	)
	fmt.Scanf("%s %d %s\n", &name, &age, &class)
    fmt.Println(name, age, class)
}
```

#### Scanln

扫描到换行符才会结束

```go
func main() {
	var(
		name string
    	age int
    	class string
	)
    fmt.Scanln(&name, &age, &class)
    fmt.Println(name, age, class)
}
```



## 类型

Go中没有float类型，只有float32和float64，当用简便方法定义一个浮点类型变量的时候，默认类型为float64

比如 f1 :=  1.2345

如果想定义一个float32类型的变量，可以使用强制类型转换

f1 := (float32)1.2345

Go是强类型

两种不同类型的float之间不可以直接相互赋值，32位给64位也不行

Go中有复数类型complex: complex64和complex128，一般用不到

Go中不允许将正数强制转换为布尔型

缺省定义布尔类型变量时默认为false

布尔类型无法参与数值运算，也无法与其他类型进行转换，这点与C语言不一样

出现在  :=  左边的变量不能是已经声明过的



## 字符串

Go语言可以定义中文字符串，因为是UTF-8编码的，定义字符串时必须用双引号包裹，单引号代表单个字符

一个ASCII字符占1个字节，一个UTF-8编码的汉字字符占3个字节，但它们都是char类型的

Go语言中的字符串以原生数据类型出现，和Java不一样

### 字符串操作

#### 1. 字符串拼接

```go
name := "avc"

world := "abcdef"

s := fmt.Sprintf("%s%s", name, world) // returns a string
```

可以用一个字符串类型的变量来接受这个函数的返回值，它把两个字符串拼接起来

也可以：

```go
ss := name + world
```

#### 2. 字符串分割

```go
ret := strings.Split(s3, "\\")
```

传入的参数是两个字符，第二个表示要分割的位置

#### 3. 字符串包含

```go
ss := "kajhdabcasdkjh
fmt.Println(strings.contains(ss, "abc")) // returns true"
```

#### 4. 前缀/后缀判断

```go
fmt.Println(strings.HasPrefix(ss, "kjh")) // returns false
fmt.Println(strings.HasSuffix(ss, "kjh")) // returns true
```

#### 5. 判断子串出现位置

```
s4 := "abcdeb"
strings.Index(s4, "c") // returns 2
strings.LastIndex(s4, "b") // returns 5 最后一次出现的位置
```

#### 6. 修改字符串

Go中的字符串无法修改，和Java一样，想要修改则需要改变成一种另外的变量

```go
s2 := "一个字符串"
s3 := []rune(s2) //把字符串强制转换成了一个rune切片
// s3 : [一 个 字 符 串]
s3[0] := '两'
string(s3) //强制类型转换回来
```

Java中用的是StringBuilder和StringBuffer

Go中强制类型转换就行了

#### 7. 判断一个字符串中含有的汉字数量

```go
unicode.Is(unicode.Han, char)
```



### 补充内容

对于ASCII码中的数据类型，称为byte

其他的如汉字或韩文称为rune（一个UTF-8字符）

int32就是rune类型，rune就是int32的别名，其中前1个字节用来标识UTF-8

Go中声明一个英文字符默认是rune类型，可以强制转换为byte类型



for range循环：增强版for循环，返回两个值，索引和索引所指向的值

```go
s := "Hello北京"
for i, v := range s {
    fmt.Printf("%d %c", i, v)
}
```

0 ~ 4 : H ~ o

5：北  

8：京



## 数组

数组的长度是数组类型的一部分，数组的长度需要在编译阶段确定

比如：[3]int [4]bool

数组声明完以后就不能变了，应用场景少一些

数组是值类型，不是引用类型



### 数组的初始化

#### 缺省初始化

如果缺省初始化，则默认元素都是零值

#### 初始化方式1：数组字面值语法

```go
a := [3]bool{true, false, true}
```

如果不知道数组的长度：根据初始值自动推断数组的长度：

```
a := [...]int{1, 2, 3, 4, 5, 6, 7, 8}
```

初始化方式2：指定索引的方式

```go
a := [5]int{0: 1, 4: 2}  --> [1, 0, 0, 0, 2]
```

### 数组遍历

#### 1. 根据索引遍历

```go
for i := 0; i < len(cities); i++ {
    fmt.Println(cities[i])
}
```

#### 2. for range

```go
for i, v := range cities {
    fmt.Println(i, v)
}
```

### 多维数组

[[1, 2], [3, 4], [5, 6]]

```go
var a = [3][2]int
a = [3][2]int{
    [2]int{1, 2},
    [2]int{3, 4},
    [2]int{5, 6},
}
```

#### 遍历多维数组

```go
for _, v := range a {
    fmt.Println(v)
    for _, v1 := range v {
        fmt.Println(2)
    }
}
```

## 切片

引入切片

```
func arraySum(x [3]int) int // 这个函数只能接受[3]int的类型，有很大的局限性
```

### 切片的定义

var s []int 

只声明类型，不声明长度

切片的底层是数组

var s1 []string

### 初始化

```go
s = []int{1, 2, 3}
s1 = []string{"沙河", "张江", "平山村"}
s == nil代表还没有为s开辟内存空间
```

### 长度和容量

切片的容量是指底层数组从切片的第一个元素到最后元素的数量

切片一定对应着一个底层数组！

由数组得到切片：

```go
a1 := [...]int{1, 3, 5, 7, 9, 11, 13}
s3 := a1[0:4] // 左闭右开
fmt.Println(s3) // [1, 3, 5, 7]
s4 := a1[1:6]
fmt.Println(s4) // [3, 5, 7, 9, 11]
```

其他格式：

```go
a1[:4]
a1[1:]   
a1[:]
s5 := a1[:4] // s5的长度是4，容量是7
```

切片再切片

```go
s6 := s4[2:] --> [7, 9, 11]
```

### 切片的本质

数组变了，切换也会跟着变，因为切片的本质就是指针 + 长度 + 容量

切片就是一个框，框住了一块连续的内存。属于引用类型，真正的数据都是保存在底层数组里的

一个nil值的切片引用并没有保存底层数组，长度和容量都是0

但长度和容量都是0的切片不一定是nil

```go
s2 := []int{} // s2 != nil
```

slice不能用==比较，slice只能和nil比较

如果要判断一个切片是不是空的，要用len(s) == 判断，而不是s == nil判断

### make函数

```go
s2 := make([]int, 0, 10) // 切片类型、长度、容量
```

### append函数

append()为切片追加元素

```go
s1 := []string{"Beijing", "Shanghai", "Shenzhen"} // 容量和长度已定
s1[3] = "Guangzhou" // index out of range索引越界
```

如何正确地追加元素？

```go
s1 = append(s1, "Guangzhou")
```

调用append函数必须用原来的切片去接收返回值

一个len == cap == 3的切片，进行append操作后len == 4, cap == 6

说明底层数组变了，自动扩容*2

为什么必须用原来的切片去接收返回值？

因为append之后切片指向的数组可能会变，追加元素，原来底层数组放不下的时候，

Go底层就会把底层数组换一个

```go
ss := []string{"Wuhan", "Xian", "Suzhou"}
s1 = append(s1, ss...) // "..."表示拆开
```

### copy函数

copy()接收两个参数，一个是dest，一个是src

拷贝后再对原数组做更改不会影响dest切片的内容

### 删除元素

Go语言中没有针对切片删除的元素，需要自己写，比如：

```go
a := [...]int{1, 2, 3}
s1 := x1[:]
s1 = append(s1[:1], s1[2:]) // 将切片s1中索引为1的元素删除
```

操作完后，s1的长度变为2，cap仍为3
底层数组被改变为{1, 3, 3}，修改了底层数组，但底层数组的位置不变

## 指针

Go中的指针比较简单，只需要记住两种操作：

& 取地址

*取值

new是用来创造指针的

一段错误的程序：

```go
var a *int // a == nil
*a = 100
```

a是一个空指针，不对应任何的内存地址

更改：

```go
var a = new(int) // 开辟一块内存
*a = 100
```

现在a对应着一块内存地址了

### make和new的区别

make只用于slice\map\chan的内存创建，make也是用来创建内存的，但是它返回的类型就是这三个类型本身，而不是他们的指针类

型。因为这三种类型本身就是引用类型

## Map

### 创建Map

map是一种无序的基于key-value的数据结构，Go中的map是引用类型，必须初始化才能使用

格式：

```go
map[key Type]value Type
```

```go
var m1 map[string]int // 还没有初始化，没有开辟内存
m1 = make(map[string]int, 10) // 要估算好该map容量，尽量避免在程序运行期间再动态扩容
m1["Beijing"] = 1000
m1["Shanghai"] = 1200
value, ok := m1["Shenzhen"] // Go中约定成俗用ok返回布尔值
if !ok {
    fmt.Println("Key does not exist")
} else {
    fmt.Println("value")
}
```

如果map中不存在这个key，拿到对应类型的零值，并不会报错

### map的遍历

```go
for k, v := range m1 {
    fmt.Println(k. v)
}
for _, v := range m1 {...}
```

### delete删除键值对

```go
delete() // 接受两个参数
delete(m1, "Beijing")
```

如果删除的key不存在，不会报错，而是什么都不做

### 按照指定顺序遍历map

```go
rand.seed(time.Now().UnixNano()) // 初始化随机数种子
var scoreMap = make(map[string]int, 200)
for i := 0; i < 100; i++ {
    key := fmt.Sprintf("stu%02d", i) // 生成stu开头的字符串
    value := read.Intn(100)  // 生成0~99的随机整数
    scoreMap[key] = value
}
// 取出map中所有的key存入切片keys
var keys = make([]string, 0, 200)
for key := range scoreMap {
    keys = append(keys, key)
}
// 对切片进行排序
sort.Strings(keys)
// 按照排序后的key遍历map
for _, key := range keys {
    fmt.Println(key, scoreMap[key])
}
```

### map 和 slice的组合

#### 元素为map类型的切片

```go
var s1 = make([]map[int]string, 10)
s[0] = make(map[int]string, 10)
s[0][100] = "Beijing" // [100 : "Beijing"]
```

#### 值为切片类型的map

```go
var m1 = make(map[string][]int, 10)
m1["Beijing"] = []int{10, 20, 30} // ["Beijing" : [10 20 30]]
```

注意：map 和 slice使用的时候一定要初始化！



## 函数

### 几种定义格式

#### 常规：

```go
func sum(x int, y int) (ret int) { // 在函数内部可以直接使用ret这个变量了
    return x + y
}
```

#### 没有返回值：

```go
func sum(x int, y int) {
    fmt.Println(x + y)
}
```

#### 没有参数没有返回值：

main函数

#### 没有参数但有返回值

```go
func f() int {
    return 3
}
```

参数可以命名也可以不命名

命名的返回值就相当于在函数中声明一个变量

第一个函数也可以这么写：

```go
func sum(x int, y int) (ret int) {
    ret = x + y
    return  // 可以不写返回值，因为已经指定
}
```

#### 多个返回值

```go
func f5() (int, string) {
    return 1, "Beijing"
}
```

接收：m, n := f()

### 参数

#### 相同类型只用说明一次

```go
func f6(x, y int) int {...}
func f7(x, y int, m, n string, i, j, k bool) {...}
```

#### 可变长参数

```go
func f7(x string, y ...int) { // y的类型是切片
    fmt.Println(x)
    fmt.Println(y)
}
```

传参：

```go
f7("北京", 1, 2, 3, 4, 5) // 北京 [1, 2, 3, 4, 5]
```

### defer语句

```go
func deferDemo() {
    fmt.Println("start")
    defer fmt.Println("123456") // defer把他后面的语句延迟到函数即将返回的时候执行
    fmt.Println("end")
}
```

多个defer语句用栈来保存，先进后出

defer多用于释放资源

在Go语言中return语句在底层并不是原子操作，它分为给返回值赋值和RET指令两步：

return x --> 1. 返回值 = x   2. RET指令

而defer语句执行的时机就是在返回值赋值操作后，RET指令执行前：

return x --> 1. 返回值 = x   2. 运行defer   3.RET指令

#### 例题1：

```go
func f1() int { // returns 5
    x := 5
    defer func() {
        x++
    }()
    return x
}

```

#### 例题2：

```
func f2() (x int) {  // returns 6，第一步返回值赋值x=5，然后执行defer语句，最后返回x++后的结果
    defer func() {
        x++
    }()
    return 5
}
```

#### 例题3：

```go
func f3() (y int) {  // returns 5
    x := 5
    defer func() {
        x++
    }()
    return x
}
```

#### 例题4：

```go
func f4() (x int) { // returns 5
    defer func() {
        x++
    }(x) // 把x当做参数传进去，传的是副本
    return 5
}
```

#### 例题5：

```go
func cal(index string, a, b int) int {
    ret := a + b
    fmt.Println(index, a, b, ret)
    return ret
}

func main() {
    a := 1
    b := 2
    defer calc("1", a, calc("10", a, b))
    a = 0
    defer calc("2", a, calc("20", a, b))
    b = 1
}
// 上面的代码输出的结果是?
/*
 * Ans:
 * 10 1 2 3
 * 20 0 2 2
 * 2 0 2 2
 * 1 1 3 4
 */
```

注意到最后一个输出中对应的a的值是1而不是0，说明执行到defer语句的时候就已经把a的值传进去了，函数中的calc同理，都是先入栈传参，所以要算出calc对应的返回值

### 变量作用域

函数中查找变量的顺序是先在函数内部查找，如果找到就用，没找到就去找全局变量，再找不到就报错

### 函数作为参数和返回值（高阶函数）

```go
func f1() {
    fmt.Println("Hello")
}

func f2() int {
    return 10
}

func main() {
    a := f1
    b := f2
    fmt.Println("%T\n", a)
    fmt.Println("%T\n", b)
    f3(f2)
}
// returns func()   func() int

func f3(x func() int) {
    ret := x()
    fmt.Println(ret)
}

func f4(x, y int) int { // type : func(int, int) int
    return x + y
}

//函数作为返回值
func ff(a, b int) int {
    return a + b
}

func f5(x func() int) func(int, int) int {
    return ff
}
```

### 匿名函数

没有名字的函数

```go
func main() {
    f1(10, 20)
    // 函数内部无法声明带名字的函数
    var f2 := func(x, y) int {
        fmt.Println(x - y)
    }
}

var f1 = func(x, y int) {
    fmt.Println(x + y)
}
```

没有名字怎么调用？ --> 把函数设置为一个变量

### 立即执行函数

如果只是调用一次的函数，还可以简写成立即执行函数

```go
func(x, y int) {
	fmt.Println("Hi!")
	fmt.Println(x + y)
}(100, 200)
```

什么时候用匿名函数？在函数内部定义函数时

什么时候使用立即执行函数？只需要调用一次的函数



### 闭包

闭包指的是一个函数和其相关的引用环境组合而成的实体。

简单来说，闭包 = 函数 + 引用环境

####  例子1：

```go
func adder() func(int) int {
    var x = 100
    return func(y int) int {
        x += y
        return x
    }
}

func main() {
    ret := adder() // returns func(int) int
    ret2 := ret(200) // returns an int
    fmt.Println(ret2) // 300
}
```

闭包就是一个函数，除了使用自己传进来的参数以外，还是用了定义它的函数里的一些外部变量

（包含了它外部作用域的一些变量）

#### 例子2：

```go
func f1(f func()) {
    fmt.Println("This is f1")
    f()
}

func f2(x, y int) {
    fmt.Println("This is f2")
    fmt.Println(x + y)
}

func main() {
    f1(f2) // 这样做类型不匹配，肯定会报错
}
```

现在我想在f1中调用f2，该怎么办？

定义一个函数对f2进行包装

```
func f3(f func(int, int)) func(){
	tmp := func() {
		f()
	}
	return tmp
}
```

在函数内部定义函数时，参数可以传递

```go
func main() {
    test(100)
}

func test(x int) {
    temp := func() {
        fmt.Println(x) // 这个x可以被找到
    }
    tmp()
}
```

那么如果传递的参数是一个函数，同样可以执行这个函数

稍加修改：

```go
func main() {
    test(f2, 100, 200)
}

func test(x func(int, int), m, n int) {
    temp := func() {
        x(m, n)
    }
    tmp() // x(m, n)
}
```

但如果我不希望立即执行呢？就可以把tmp设为返回值

```go
func main() {
    ret := test(f2, 100, 200)
    ret() // 把调用时机改到这个位置
}

func test(x func(int, int), m, n int) func() { 
    temp := func() {
        x(m, n)
    }
    return tmp // 把括号去掉，不立即执行
}
```

输出结果：

This is f2 

300

在main()中调用ret，有点类似于调用f2

test这个函数包装了f2，现在我们可以在f1中调用test函数的返回值从而达到调用f2的目的了

总结：

现在我们想要在f1中调用f2，但由于参数类型不匹配，无法直接调用，所以我们需要利用闭包

```go
func f1(f func()) {
    fmt.Println("This is f1")
    f()
}

func f2(x, y int) {
    fmt.Println("This is f2")
    fmt.Println(x + y)
}

func f3(f func(x, y int), x, y int) func() {
    // temp是一个没有参数也没有返回值的匿名函数，和f1中要求的参数类型相匹配，但是temp可以拿到x, y两个变量
    tmp := func() { 
        // 在这里调用f2，f2作为参数传递进来
        f(x, y)
    }
    return tmp
}

func main() {
    ret := f3(f2, 100, 200) // 参数是匹配的
    f1(ret)
}
```

效果：把原来一个需要两个int类型参数的函数包装成一个不需要传参的函数

#### 例子3：

```go
func makeSuffixFunc(suffix string) func(string) string {
    return func(name string) string {
        if !strings.HasSuffix(name, suffix) {
            return name + suffix
        }
        return name
    }
}

func main() {
    jpgFunc := makeSuffix(".jpg")
    txtFunc := makeSuffix(".txt")
    fmt.Println(jpgFunc(test))  // test.jpg
    fmt.Println(txtFunc(test))  // test.txt
}
```

#### 例子4：

```go
func calc(base int) (func(int) int, func(int) int) {
    add := func(i int) int {
        base += i
        return base
    }
    sub := func(i int) int {
        base -= i
        return base
    }
    return add, sub
}

func main() {
    f1, f2 := calc(10)
    fmt.Println(f1(1), f2(2)) // 11 9
    fmt.Println(f1(3), f2(4)) // 12 8
    fmt.Println(f1(5), f2(6)) // 13 7
}
```



#### 底层原理：

1. 函数可以作为返回值
2. 函数内部查找变量的顺序，先在自己内部找，找不到往外层找

### Go中的几个内置函数

|    内置函数    |                             介绍                             |
| :------------: | :----------------------------------------------------------: |
|     close      |                     主要用来关闭channel                      |
|      len       |        用来求长度，比如string\array\slice\map\channel        |
|      new       | 用来分配内存，主要用来分配值类型，比如int\struct，返回的是指针 |
|      make      |    用来分配内存，主要用来分配引用类型，比如chan\map\slice    |
|     append     |                 用来追加元素到数组、slice中                  |
| panic和recover |                        用来做错误处理                        |

#### panic/recover

Go语言目前没有异常机制，但是使用panic/recover模式来处理错误。panic可以在任何地方引发，但recover只有在defer调用的函数中有效。

```go
func funcA() {
    fmt.Println("a")
}

func funcB() {
    // 刚刚打开数据库连接，出现了严重错误
    defer func() {
        err := recover()
        fmt.Println(err)
        fmt.Println("释放数据库连接...")
    }()
    panic("出现了严重的错误！")
    fmt.Println("b")
}

func funcC() {
    fmt.Println("c")
}

func main() {
    funcA()
    funcB()
    funcC()
}

输出：
a
出现了严重的错误！
释放数据库连接...
c // funcC可以照常执行，也不会有报错
```

在实际代码中，不要滥用recover()函数，因为一个项目中如果连数据库都没有成功连接那么剩下的代码将毫无意义

该panic就该panic

defer一定要在可能引发panic的语句之前定义，因为panic之后的语句执行不到(unreachable)



## 结构体

```go
func f1(x person) { // Go语言中所有是参数都是拷贝
    x.gender = "male" // 修改的是拷贝的值，不会对原值产生影响
}

func f2(x *person) {
    (*x).gener = "male"
    x.gender = "male"  // 这两者在Go语言中的语义上是等价的，因为Go语言并不支持对指针的修改，所以修改的是指针指向的变量。这是Go语言提供的语法糖
}
```



我们还可以用new关键字对结构体进行实例化，得到的是结构体地址，格式如下：

```go
var p2 = new(person)
fmt.Printf("%T\n", p2) // *main.person

```



## Gorountine

### context

如何通知子gotoutine结束？

方法一：设置全局变量

方法二：使用channel

方法三：使用context



背景：

在net/http包中，每收到一个http请求，都会开启一个goroutine区处理，其中，有的goroutine可能会调度别的事务比如查询数据库，这样就会开启更多的goroutine，在这样层层调度的场景下，如何判断最初的请求是否超时了呢？

比如，如果试图连接数据库，500ms后还没有连接上就判断为超时

土办法：自己造一个定时器，定时器结束计时后向channel中传递一个消息 -> 每个程序员的解决方案和代码风格会不一样，导致开发协同困难

于是诞生了context，是官方为了解决这个问题提出的统一解决方案



使用context解决这个问题：

```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)

var wg sync.WaitGroup

func f(ctx context.Context)  {
	defer wg.Done()
	for {
		fmt.Println("Demo testing")
		time.Sleep(time.Millisecond * 500)
		select {
		// ctx.Done <- chan struct{} : a read only channel
		case <- ctx.Done():
			break
		default:
		}
	}
}

func main() {
	// 返回的第二个参数是CancelFunc类型
	// type CancelFunc func() --> 没有参数，也没有返回值
	// context是一级一级往下传的，最开始传入的是context.Background()
	ctx, cancel := context.WithCancel(context.Background())
	go f(ctx)
	time.Sleep(time.Second * 5)
	cancel()
	wg.Wait()
}

```

