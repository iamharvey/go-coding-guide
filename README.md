# Go语言编码规范 

<br>

| 作者 | 仰山 |
| -- | -- |
| 当前版本 | 1.0 |
| 上一次更新 | 2021-09-18 |


<br><br>

###  摘要
这是一篇关于如何规范和优雅地使用Go语言进行编码工作的文档。文档主要参考官方推荐文档[《 Effective Go》](https://golang.org/doc/effective_go)、[《Go Code Review Comments》](https://github.com/golang/go/wiki/CodeReviewComments) 进行编写，同时采纳了[《Uber Go Style Guide》](https://github.com/uber-go/guide/blob/master/style.md)、William Kenndy 和 Hoanh 在《Ultimate Go Notebook》中许多关于编写高质量Go代码的建议。文档旨在缩短新晋Go工程师的上手周期，为他们提供基于不同约束力（例如：强制的、可参考的）的规范性指导。同时，文档的第三至八章还为提供了一些（软件）工程实践的指导。

<br>

<hr>

<br>

## 目录

- [1. 介绍](#1-%E4%BB%8B%E7%BB%8D)
  - [1.1 阅读引导](#11-%E9%98%85%E8%AF%BB%E5%BC%95%E5%AF%BC)
  - [1.2 名词对照表](#12-%E5%90%8D%E8%AF%8D%E5%AF%B9%E7%85%A7%E8%A1%A8)
  - [1.3 文档组织](#13-%E6%96%87%E6%A1%A3%E7%BB%84%E7%BB%87)
- [2. 编程规约](#2-%E7%BC%96%E7%A8%8B%E8%A7%84%E7%BA%A6)
  - [2.1 命名规则（Naming）](#21-%E5%91%BD%E5%90%8D%E8%A7%84%E5%88%99naming)
  - [2.2 变量与常量（Variables and Constants）](#22-%E5%8F%98%E9%87%8F%E4%B8%8E%E5%B8%B8%E9%87%8Fvariables-and-constants)
  - [2.3 代码格式（Formatting）](#23-%E4%BB%A3%E7%A0%81%E6%A0%BC%E5%BC%8Fformatting)
  - [2.4 注释（Comment）](#24-%E6%B3%A8%E9%87%8Acomment)
  - [2.5 控制语句（Control）](#25-%E6%8E%A7%E5%88%B6%E8%AF%AD%E5%8F%A5control)
  - [2.6 结构体（Struct）](#26-%E7%BB%93%E6%9E%84%E4%BD%93struct)
  - [2.7 常用数据结构（Array,  Slice And Map）](#27-%E5%B8%B8%E7%94%A8%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84array--slice-and-map)

<br>

<hr>

<br>

## 1. 介绍

<br>

这一部分是文档的介绍。其中包含了阅读引导、名词对照，并对文档的组织结构进行了说明。

<br>

### 1.1 阅读引导

为了便于在实际工作中开展实践，文档对规约的约束力进行区分。为此，文档参考了《阿里巴巴Java开发手册》后，也将所有规约分为三个等级：
- **【强制】** - 必须遵从的规约。他们是代码入库的门禁；
- **【推荐】** - 推荐的规约。遵循这些规约能编写出更好、更优雅的代码，但不作为入库门禁；
- **【参考】** - 可以参考的规约。是否采纳需要结合实际情况而定，也不作为入库门禁。

<br>

为了便于记忆，文档尽可能让规约的描述短小精悍。为此，文档通过添加【说明】部分来展现对规约的补充说明，如下所示：

```
【说明】
<补充说明内容>
```

<br>

同时，文档增加了范例，来更好地阐释规约。其中：
- **GOOD**  - 为规约要求或推荐的方式；
- **BAD** - 为规约禁止或不提倡的方式。

<br>

例如：

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
s := "foo"

```

</td><td>

```go
var s = "foo"

```

</td></tr>
</tbody></table>

<br>

文档中每个规约的编号都是全局唯一的。这样做的目的是方便大家进行全局的检索。规约编号为数字格式“x.x”，规约描述以“【规约 x.x】- <规约内容>”呈现。例如：

> 👉【规约2.1】【强制】-  避免使用可被修改的全局变量。


<br>

### 1.2 名词对照表
为了避免因主观翻译造成理解上的偏差，对一些没有把握的翻译，通常会在其后附上原生英文名称。对于一些特异性的概念，如“goroutine”、“channel”等并未做出翻译。因为在平时的工作和学习中，大家都会经常使用这些原生英文交流，若再刻意进行翻译，倘若翻译不当，反而影响阅读流畅性。由此出现中英文混用的情况，望大家谅解。

<br>

文中经常出现的一些名称及其原生英文的对照关系如下表：

| **中文名称**    | **原生英文名称**              |
|-------------|-------------------------|
| 包           | package                 |
| （包）导入       | import                  |
| （包）别名       | alias                   |
| 类型          | type                    |
| 接口          | interface               |
| 结构体         | struct                  |
| 值           | value                   |
| 指针          | pointer                 |
| 接收者（方法中的概念） | receiver                |
| 值类接受者       | value receiver          |
| 指针类接受者      | pointer receiver        |
| 申明          | declaration             |
| （变量）引用      | reference               |
| 字符串         | string                  |
| 字节          | byte                    |
| 数组          | array                   |
| 切片          | slice                   |
| 上下文         | context                 |
| 崩溃          | panic / panicing        |
| 堆           | heap                    |
| 栈           | stack                   |
| 垃圾回收        | garbage collection (gc) |
| 返回值         | returned value(s)       |
| 并发          | concurrency             |
| 互斥锁         | mutex lock              |
| 原子操作        | atomic operation        |
| 性能测试        | Benchmark testing       |

<br>

### 1.3 文档组织

文档的组织结构如下：
- Section 2 “编程规约” - 对 Go 语言编程的基本实践方法做出了约定。这些约定包括：命名规则、变量与常量的定义、代码格式、注释、控制语句的使用、数据结构的使用、时间的处理、函数及方法的使用、上下文处理、接口的定义和使用、并发处理、错误处理、崩溃及恢复处理以及一些实用的与性能相关的建议；
- Section 3 “测试” - 对如何编写高质量测试做出了约定；
- Section 4 “日志” - 对如何输出日志做出了约定；
- Section 5 “安全” - 对如何保障代码及软件安全的方法作出了约定；
- Section 6 “工程结构” - 对如何使用 Go 语言编写微服务作出了约定；
- Section 7 “门禁检查自动化” - 对门禁检查的自动化执行的基本步骤作出了约定。

<br>

<hr>

<br>

## 2. 编程规约

<br>

### 2.1 命名规则（Naming）

>
> “There are only two hard things in Computer Science: 
>  
>     cache invalidation and naming things.”
>
>                                       — Phil Karlton


<br>

👉【规约 1.1】【强制】- 包（package）的命名，请遵循以下原则：
1. 使用lower-case，不允许使用下划线；
2. 简明扼要，不宜过长；
3. 不要使用复数；
4. 不要使用非常泛化的名词，例如：“common”, “util”, “shared”, 或 “lib”，信息量太少了；
5. 不要使用“.”。

```text
【说明】
不同的包中可能包含同名的类型值、方法或函数，使用“.”后，我们无法直观了解所导入的到底来自哪个包 ，降低了代码可读性。
```

<br><br>

👉【规约1.2】【强制】- 变量、 常量、函数及方法的命名遵循“mixedCaps”或“MixedCaps”风格，由英文字母组成。

```
【说明】
- “mixedCaps”适用于不需要暴露的变量、常量、函数及方法；
- “MixedCaps”适用于需要暴露的变量、常量、函数及方法；
- 应尽量减少包的暴露面积。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
const (
    debugMode = true
)

var userName string

type clientConfig struct {}

type ServerConfig struct {}

func ResolveValue(str string) string { 
    ... 
}    

```

</td><td>

```go
const (
    // 不要使用下划线连接word。
    DEBUG_MODO = true
)

var (
    // 变量中不要带有中文或其他文字。
    user张三 *User
    
    // 不要使用数字，尽量提高变量的自解释性。
    user1 *User
    user2 *User
)

func User_Get(id string) *User { 
    ... 
}

```

</td></tr>
</tbody></table>

<br><br>

👉【规约 1.3】【强制】- 代码和注释中都要避免使用任何脏话、侮辱性及涉及歧视和偏见词汇。  

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
strs := []string{"日本人", "印度人"}
blockList := []string{"1.2.3.4", "2.3.4.5"}
allowedList := []string{"192.168.1.1/24", "172.1.1/20"}
```

</td><td>

```go
blackList := []string{"1.2.3.4", "2.3.4.5"}
whiteList := []string{"192.168.1.1/24", "172.1.1/20"}
pool := newSlavePool(...)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约1.4】【强制】- 如果接口中仅定义了一个函数，接口名应为：函数名 + “er”或“or”。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// Authenticator defines an authenticator that is 
// able to validates authorization tokens.
type Authenticator interface {
    Authenticate(string) (*Claims, error)
}
```

</td><td>

```go
// Auth defines an authenticator that is able to 
// validates authorization tokens.
type Auth interface {
    Authenticate(string) (*Claims, error)
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约1.5】【强制】- 避免使用 build-in 名称来给变量、常量、函数和方法等命名。

```text
【说明】
Go团队在其 官方文档 中列举了 build-in 预定义的标识符。我们需要避免使用这些名称。这些名称不仅容易在阅读代码时造成混淆，还容易引起bug。
我们不要期望编译器来识别这样的问题（不过大部分IDE都会在编码时进行提示），因为大部分情况下，编译并不会出错。需要主观上避免。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type Foo struct {
    // 'err' and 'str' 
    // 不会与 build-in 名称 'error', 'string'
    // 发生混淆。
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}     
```

</td><td>

```go
type Foo struct {
    // 'error' and 'string' 
    // 与 build-in 名称 'error', 'string'
    // 发生混淆。
    error  error
    string string
}

func (f Foo) Error() error {
    // 'error', 'f.error] 在视觉上很相似。
    return f.error
}

func (f Foo) String() string {
    // 'string' 和 'f.string' 在视觉上很相似。
    return f.string
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约1.6】【强制】- 如果包名和导入的包路径的最后一部分不匹配，应使用别名（alias）。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
import (
    "net/http"
    "runtime/trace"

     client "example.com/client-go"
     nettrace "golang.net/x/trace"
)
```

</td><td>

```go
import (
    "net/http"
    "runtime/trace"

    // 没有使用别名。
    "example.com/client-go"
    
    // 糟糕的别名。
    trace2 "example.com/trace/v2"
)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约1.7】【推荐】- 在为结构体（struct）定义成员方法（methods）时：“get”类方法命名省略使用“Get”前缀；set类方法需使用“Set” 前缀。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type Pet struct {
    name string
    owner string
}

...

func (p *Pet) Owner() string {
    return p.owner
}

func (p *Pet) SetOwner(owner string) {
    p.owner = owner
}

func main() {
    p := &Pet{
        name: "啸天犬",
    }
    
    p.SetOwner("杨戬")
    fmt.Printf("%s's owner is %s", p.Name(), p.Owner())
}
```

</td><td>

```go
type Pet struct {
    name string
    owner string
}

...

func (p *Pet) GetOwner() string {
    return p.owner
}

func (p *Pet) SetOwner(owner string) {
    p.owner = owner
}

func main() {
    p := &Pet{
        name: "啸天犬",
    }
    
    p.SetOwner("杨戬")
    fmt.Printf("%s's owner is %s", p.GetName(), p.GetOwner())
}
```

</td></tr>
</tbody></table>

<br><br><br>

### 2.2 变量与常量（Variables and Constants）

<br>

👉【规约2.1】【强制】-  避免使用可被修改的全局变量。

```
【说明】
这类全局变量的问题就在于：它会在你不知情的情况下，被改变。
````

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type signer struct {
    now func() time.Time
}

func newSigner() *signer {
    return &signer{
        now: time.Now,
    }
}

func (s *signer) Sign(msg string) string {
    now := s.now()
    return signWithTime(msg, now)
}
```

</td><td>

```go
var _timeNow = time.Now

func sign(msg string) string {
    now := _timeNow()
    return signWithTime(msg, now)
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约2.2】【强制】-  定义多个常量，通过小括号( )类聚。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// Database migration settings.
const (
   modeDebug               = false
   migrateDropIndex        = false
   migrateDropColumns      = false
   migrateWithFKConstrains = true
   enableHooks             = true
)
```

</td><td>

```go
const modeDebug = false
const migrateDropIndex = false

func NewData(...) { 
    ... 
}

const migrateDropColumns = false
const migrateWithFKConstrains = true

const enableHooks = true

var hook func(next ent.Mutator) ent.Mutator { 
    ... 
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约2.3】【强制】-  避免使用“magic word”。

```
【说明】
“magic word”是那些没有通过变量/常量名来使用“裸值”，通常也没有注释。这些“毫无征兆”地出现的“magic word”有时会让准确理解代码变得困难。
```


<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
const (
    numOfWorkers = 4
    stopOnErr = true
)
...
n, err := runJobs(jobs, numOfWorkers, stopOnErr)
```

</td><td>

```go
n, err := runJobs(jobs, 4, true)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约2.4】【强制】-  使用“raw string literals”方式定义字符串（string），防止“hand-escaped”的字符。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := `unknown error:"test"`
```

</td><td>

```go
wantError := "unknown name:\"test\""
```

</td></tr>
</tbody></table>

<br><br>

👉【规约2.5】【推荐】-  建议使用:=来定义本地变量。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
s := "foo"
```

</td><td>

```go
var s = "foo"
```

</td></tr>
</tbody></table>

<br>

在某些情况下，使用var会让默认值看起来更清楚：

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
    var filtered []int
    for _, v := range list {
        if v > 10 {
            filtered = append(filtered, v)
        }
    }
}
```

</td><td>

```go
func f(list []int) {
    filtered := []int{}
    for _, v := range list {
        if v > 10 {
            filtered = append(filtered, v)
        }
    }
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约2.6】【推荐】-  在枚举常量时，使用= iota来省略等差递进赋值：

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// ErrCode - error code.
type Status int

// Common error codes.
const (
   StatusUnknown Status = iota + 1
   StatusStart
   StatusStop
   StatusPending
   ...
)
```

</td><td>

```go
// Common error codes.
const (
   StatusUnknown = 1
   StatusStart = 2
   StatusStop = 3
   StatusPending = 4
   ...
)
```

</td></tr>
</tbody></table>

<br><br><br>

### 2.3 代码格式（Formatting）

<br>

👉【规约3.1】【强制】- 如果是大括号内为空，则简洁地写成{}即可，大括号中间无需换行和空格；如果是非空代码块则需遵循以下原则：
- 左大括号前不换行；
- 左大括号后换行；
- 右大括号前换行；
- 表示终止的右大括号后必须换行。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// 这样定义是ok的，但我们在实际工作中
// 不需要这样的空方法。毫无意义！
func g() {}

func main() {
    ...
    if i < f() {
        g()
    }
}
```

</td><td>

```go
// g不为空，但左大括号后没有换行。
func g() { fmt.Println("say hi from g") }

// 左大括号前无需换行。
if i < f()
{
    g()
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约 3.2】【强制】- 任何二目、三目运算符（包括赋值运算符=、逻辑运算符&&、加减乘除符号）的左右两边都需要加一个空格。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func f(a, b int) bool {
    return (a + b) == 8
}
```

</td><td>

```go
func f(a, b int) bool {
    return (a+b)==8
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约 3.3】【强制】- 使用 Tab 字符（4个空格）缩进。 

```
【说明】
gofmt默认也适用Tab字符来缩进。在文档的范例代码中，都是用 Tab 字符（4个空格）缩进。
```

<br><br>

👉【规约3.4】【强制】- 单行字符数限制不超过120个，超出需要换行。换行时遵循如下原则：
- 第二行相对第一行缩进一个Tab；
- 运算符不与下文一起换行；
- 函数调用的.符号不与下文一起换行；
- 函数调用中的多个参数需要换行时，在逗号后换行；
- 在括号前不要换行。 

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// Run executes the command. 
func (o *Options) Run() error {
    fromCache := false
    t := time.Now()

    // Fetch api resources from API server.
    res, err := resource.NewBuilder(fromCache, o.Config).
        Resource(defaultResourceType, "").
        Schema(defaultSchema).
        URL(defaultAPIPath).
        CacheUp(defaultCacheUp).
        SaveAs(defaultDictName, resource.MakeDict).
        Do()
    ...
}
```

</td><td>

```go
res, err := resource.NewBuilder(fromCache, o.Config).
Resource(defaultResourceType, "") // 没有缩进
.Schema(defaultSchema)  // 调用的点符号与下文一起换行了
.URL(defaultAPIPath)
.CacheUp(defaultCacheUp)
.SaveAs(defaultDictName 
, resource.MakeDict).Do() // 在逗号前换行了
```

</td></tr>
</tbody></table>

<br><br>

👉【规约3.5】【强制】- 单个函数/方法传入参数不超过6个。

<br><br>

👉【规约3.6】【强制】- 函数/方法参数在定义和传入时，多个参数逗号后面必须加空格。 

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func f(a, b int) int {
    return a + b
}

func main() {
    a := 1
    b := 2
    c := func(a, b)
}
```

</td><td>

```go
func f(a,b int) int {
    return a + b
}

func main() {
    a := 1
    b := 2
    c := func(a,b)
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约3.7】【强制】- 单个函数返回值数量不超过3个。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func subtract(a, b int) int {
    return a - b
}

func sum(a, b int) int {
    return a + b
}

...

func main() {
    a := 1
    b := 2
    diff := subtract(a, b)
    sum  := sum(a, b)
    ...
}
```

</td><td>

```go
// calc 干了太多活。
func calc(a, b int) (int, int, int, bool) {
    return a - b, a + b, a * b, (a + b) == 8
}

func main() {
    a := 1
    b := 2
    sub, sum, mul, is8 := calc(a, b)
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约3.8】【强制】- IDE的“text file encoding”设置为 UTF-8；IDE中换行符使用Unix格式，不要使用Windows格式。 

<br><br>

👉【规约3.9】【推荐】- 单个函数/方法不超过80行。

```
【说明】
- 除注释之外的函数/方法签名、左右大括号、方法内代码、空行、回车及任何不可见字符的总行数不超过80行；
- 代码逻辑分清红花和绿叶，个性和共性，绿叶逻辑单独出来成为额外方法，使主干代码更加清晰；
- 共性逻辑抽取成为共性方法，便于复用和维护，也更便于编写单元测试。
```

<br><br>

👉【规约3.10】【推荐】- 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。 

<br><br>

👉【规约3.11】【推荐】- 使用()来类聚相似的申明（declaration）。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
const (
    a = 1
    b = 2
)

var (
    c = 1
    d = 2
)

type (
    Area float64
    Volume float64
)
```

</td><td>

```go
const a = 1
const b = 2

var c = 1
var d = 2

type Area float64
type Volume float64
```

</td></tr>
</tbody></table>

<br><br>

👉【规约3.12】【推荐】- 组织导入（import）申明要遵循这样的顺序：“先标准库，再其他”。同源的包申明要组织在一起。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
import (
    "fmt"
    "os"

    "go.uber.org/atomic"
    "golang.org/x/sync/errgroup"
)
```

</td><td>

```go
import (
    "fmt"
    "os"
    "go.uber.org/atomic"
    "golang.org/x/sync/errgroup"
)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约3.13】【推荐】- 在提交代码前，运行gofmt来自动修正格式问题。

```
【说明】
可以使用格式化自动化工具并不代表你不需要了解编码风格的规约，了解和掌握它们对培养良好的编码习惯、编写出“令人愉悦”的代码至关重要。
```

<br><br><br>

### 2.4 注释（Comment）

<br>

👉【规约4.1】【强制】- 对包进行注释，要使用/* 内容 */格式。并以“package <包名称>”开头。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
/*
Package regexp implements a simple library for regular
expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```

</td><td>

```go
// This package implements ....
// ...
package regexp
```

</td></tr>
</tbody></table>

<br><br>

👉【规约4.2】【强制】- 所有对外暴露的变量、常量、函数或方法都需要添加注释。添加注释要以 `//` 开头，并以变量、常量、函数或方法名开头，注释的双斜线与注释内容之间有且仅有一个空格。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// Compile parses a regular expression and returns, 
// if successful, a Regexp that can be used to match 
// against text.
func Compile(str string) (*Regexp, error) {
	...
}
```

</td><td>

```go
// This function parses ...
func Compile(str string) (*Regexp, error) {
	...
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约4.3】【强制】- 注释必须是一个完整的句式（拥有完整的句式）。

```
【说明】
拥有完整的句式，可以让他们在生成doc时，具备良好的可读性。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// NewUser creates an new user.
func NewUser(*User) (*User, error) {
    ...
}
```

</td><td>

```go
// NewUser, new user.
func NewUser(*User) (*User, error) {
	...
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约4.4】【推荐】- 与其“半吊子”英文来注释，不如用中文注释把问题说清楚。专有名词与关键字保持英文原文即可。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// healthCheck 检查上游服务的心跳状态。
// 如果心跳正常，返回true；反之，返回false。 
func healthCheck(ctx context.Context) bool {
    ...
}
```

</td><td>

```go
// healthCheck 检查up服务的state.
// 如果state ok, 返回 true; 反之 false。
func healthCheck(ctx context.Context) bool {
    ...
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约4.5】【推荐】- 与其“半吊子”英文来注释，不如用中文注释把问题说清楚。专有名词与关键字保持英文原文即可。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// healthCheck 检查上游服务的心跳状态。
// 如果心跳正常，返回true；反之，返回false。 
func healthCheck(ctx context.Context) bool {
    ...
}
```

</td><td>

```go
// healthCheck 检查up服务的state.
// 如果state ok, 返回 true; 反之 false。
func healthCheck(ctx context.Context) bool {
    ...
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约4.6】【推荐】- 代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑等的修改。

```go
【说明】
代码与注释更新不同步，就像路网与导航软件更新不同步一样，如果导航软件严重滞后，就失去了导航的意义。
```

<br><br>

👉【规约4.7】【推荐】- 对于还没有搞定，仍然存在问题的代码要添加FIXME注释（第二行可以考虑缩进一个Tab字符）。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// NewWorkerPool returns a worker pool.
// FIXME：The function has data race problem, 
//        need to be fixed in v1.0.2.
func NewWorkerPool(opt *PoolOption) (*Pool, err) {
    ...
}
```

</td><td>

```go
// TLSClientConfig defines a client TLS config.
type TLSClientConfig struct {
    CACert     string
    ServerName string
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约 4.8】【参考】- 谨慎注释掉代码。在上方详细说明，而不是简单地注释掉。如果无用，则删除。

```text
【说明】
代码被注释掉有两种可能性：
1. 后续会恢复此段代码逻辑；
2. 永久不用。

前者如果没有备注信息，难以知晓注释动机。后者建议直接删掉即可。
```

<br><br>

👉【规约 4.9】【参考】- 注释要准确和清晰。

```text
【说明】
对于注释的要求：
1. 能够准确反映设计思想和代码逻辑；
2. 能够描述业务含义，使阅读代码的同学能够快速了解到代码编写的背景。
```

<br><br>

👉【规约 4.10】【参考】- 对于自解释性好的本地函数、变量，可不添加注释。

```text
【说明】
好的命名、代码结构是自解释的。对于那些看到名称就可以较容易明白其作用或功能的常量、变量、函数或方法可不添加注释。这里需要注意的是：对外暴露的部分都需要加注释，因为他们会成为API文档的一部分。
```

<br><br>

👉【规约 4.11】【参考】- 特殊注释标记，请注明标记人与标记时间。注意及时处理这些标记，通过标记扫描，经常清理此类标记。

```text
【说明】
线上故障有时候就是来源于这些标记处的代码：
- 待办事宜（TODO）:（标记人，标记时间，[预计处理时间]） 表示需要实现，但目前还未实现的功能；
- 错误，不能工作（FIXME）:（标记人，标记时间，[预计处理时间]） 在注释中用FIXME标记某代码是错误的，而且不能工作，需要及时纠正的情况。 
```

<br><br><br>

### 2.5 控制语句（Control）

<br>

👉【规约5.1】【推荐】- 表达异常的分支时，少用“if-else”的方式。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
err := doSomething()
if err != nil {
    return nil, err
}

doSomethingElse()
```

</td><td>

```go
err := doSomething()
if err == nil {
	doSomethingElse()
} else {
    return nil, err
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.2】【推荐】- 尽量避免“if-else”语句的多层嵌套。

```text
【说明】
通过对控制流的重新梳理，一般我们都可以将嵌套移除，这样可以大大增加代码可读性，让debug也变得容易些。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
for _, v := range data {
	if v.F1 != 1 {
		log.Printf("Invalid v: %v", v)
        continue
    }
  
    v = process(v)
    if err := v.Call(); err != nil {
		return err
    }
    v.Send()
}
```

</td><td>

```go
for _, v := range data {
    if v.F1 == 1 {
        v = process(v)
        if err := v.Call(); err == nil {
            v.Send()
        } else {
            return err
        }
    } else {
		log.Printf("Invalid v: %v", v)
    }
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.3】【推荐】- 如果返回值仅为错误，通常可以将错误定义成局部变量判断和使用。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
if err := file.Chmod(0664); err != nil {
    return err
}
```

</td><td>

```go
err := file.Chmod(0664)
if err != nil {
    return err
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.4】【推荐】- 如果返回值包含了除错误值外的其他值，且这些返回值后续还需使用，则需将错误检查换行书写。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```

</td><td>

```go
if f, err := os.Open(name); err != nil {
    return err
}

// 会出现编译错误，因为'f'为本地变量，
// 对外部代码不可见。
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.5】【推荐】- 遍历字符串，数组（array）， 切片（slice），map 或从 channel 中读取数据时，请使用range。
<br>

<table>
<thead><tr><th style="color:green;">GOOD</th></tr></thead>
<tbody>
<tr><td>

```go
// Copy a map.
for key, value := range oldMap {
    newMap[key] = value
}

// Read a string char by char.
for pos, char := range "中国崛起，世界和平" {
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```

</tr>
</tbody></table>

<br><br>

👉【规约5.6】【推荐】- 如果仅遍历字符串，数组，切片，map 或 channel 中的值，而无需使用index值或key值时，使用`_`来忽略它们。
<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
sum := 0
for _, v := range array {
    sum += value
}
```

</td><td>

```go
sum := 0
for k, v := range array {
    sum += value
    // 'k' is not used.
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.7】【推荐】- 在使用switch语句时，相同的case建议合并。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
select {
    case sig := <-sigChan:
    switch sig {
	case syscall.SIGHUP:
        if err := srv.forkChild(killMaster, mpid); err != nil {
            continue
        }
        return srv.shutDown()
    case syscall.SIGUSR1, syscall.SIGUSR2:
        if err := srv.forkChild(killMaster, mpid); err != nil {
            continue
        }
    case syscall.SIGINT, syscall.SIGTERM:
        return srv.shutDown()
	}
}
```

</td><td>

```go
select {
    case sig := <-sigChan:
    switch sig {
    case syscall.SIGHUP:
        if err := srv.forkChild(killMaster, mpid); err != nil {
            continue
        }
        return srv.shutDown()
    case syscall.SIGUSR1:
        if err := srv.forkChild(killMaster, mpid); err != nil {
            continue
        }
    case syscall.SIGUSR2:
        if err := srv.forkChild(killMaster, mpid); err != nil {
            continue
        }
    case call.SIGINT, syscall.SIGTERM:
        return srv.shutDown()
    }
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约5.8】【推荐】- 在使用switch语句时，break默认退出 switch block。在多控制语句嵌套使用时，可以使用break <block名称>来退出指定的 block。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th></tr></thead>
<tbody>
<tr><td>

```go
Loop:
    for n := 0; n < len(src); n += size {
        switch {
            case src[n] < sizeOne:
                if validateOnly {
                    break
                }
                size = 1
                update(src[n])
            case src[n] < sizeTwo:
                if n+1 >= len(src) {
                    err := errShortInput
                    break Loop // 退出整个'Loop' block。
                }

                if validateOnly {
                    break
                }

                size = 2
                update(src[n] + src[n+1]<<shift)
            }
        }
    }
```

</tr>
</tbody></table>

<br><br><br>

## 2.6 结构体（Struct）

<br>

👉【规约6.1】【强制】-  使用“composite literals” 来初始化结构体。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
// heads is a two-dimensional slice
var heads = []*[4]byte{
    {'P', 'N', 'G', ' '},
    {'G', 'I', 'F', ' '},
    {'J', 'P', 'E', 'G'},
}
```

</td><td>

```go
func NewFile(fd int, name string) *File {
  if fd < 0 {
      return nil
  }
  f := new(File)
  f.fd = fd
  f.name = name
  f.dirinfo = nil
  f.nepipe = 0
  return f
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约6.2】【强制】-  在初始化结构体时，总是使用“field name”。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{
    FirstName: "敖",
    LastName: "丙",
    Admin: true,
}
```

</td><td>

```go
k := User{"敖", "丙", true}
```

</td></tr>
</tbody></table>

<br><br><br>

## 2.7 常用数据结构（Array,  Slice And Map）

<br>

👉【规约7.1】【强制】-  当我们需要获得一个nil值的切片时，需使用var来定义变量。

```text
【说明】
- :=返回的是一个非空的 zero-length的切片；
- 在某些情况下，我们会倾向使用:=而非var。例如：在将 一个对象编码成一个JSON对象时，空切片将会被编码成nil。这时，我们需要使用[]string{}将它编码成JSON空数组[]。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
var a []int
...
if a == nil {
    ...
} 
```

</td><td>

```go
a := []int{}
...
if a == nil { // 'a'不会为nil值。
    ...
} 
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.2】【强制】-  在使用copy函数来复制数组或切片时，不强制要求两个数组或切片的类型必须一样 ，但需要复制的元素类型必须是一样的。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type (
    Ta []int
    Tb []int
)

dest := Ta{1, 2, 3}
src := Tb{5, 6, 7, 8, 9}

n := copy(dest, src)
fmt.Println(n, dest) // 3 [5 6 7]

n = copy(dest[1:], dest)
fmt.Println(n, dest) // 2 [5 5 6] 
```

</td><td>

```go
type (
    Ta []int
    Tb []int32
)

dest := Ta{1, 2, 3}
src := Tb{5, 6, 7, 8, 9}

// 编译错误，元素类型不一致
// dest为int，而src为int32。
n := copy(dest, src)
fmt.Println(n, dest) 
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.3】【强制】-  当使用=来拷贝数组或切片时，需确保类型是一致的。
<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type Ta []int

var newOne Ta
oldOne := Ta{1, 2, 3}
newOne = oldOne
fmt.Printf("%+v\n", newOne) // [1 2 3]
```

</td><td>

```go
type (
    Ta []int
    Tb []int
)

ta := Ta{1, 2, 3}
// 编译错误，类型不一致
// 'ta' 的类型为Ta，'tb'的类型为Tb
tb := ta
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.4】【强制】-  在遍历切片时，使用“value semantics”。

```text
【说明】
遍历数组或切片时，通常有两种方式：
- “value semantics” - 读和写的操作都通过拷贝进行；
- “pointer semantics” - 读和写的操作都通过指针进行。

通过“value semantics”来遍历数组或切片的好处是：
- 数据的读写操作本地化，对外部函数/方法不可见，隔离性好；
- 由于没有引用（reference），数据不会“逃逸”到堆上，减少垃圾回收（GC)的性能开销。

只有一种情况下，可以考虑使用“pointer semantics”：共享切片进行decoding或unmarshalling操作。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>
<p style="text-align: center;">Value Semantics</p>

```go
fruits := [3]string{"Apple", "Orange", "Banana"}
for i, fruit := range fruits {
    println(i, fruit)
}

// Output:
// 0 Apple
// 1 Orange
// 2 Banana
```

</td><td>
<p style="text-align: center;">Pointer Semantics</p>

```go
fruits := [3]string{"Apple", "Orange", "Banana"}
for i := range fruits {
    println(i, fruits[i])
}
 
// Output:
// 0 Apple
// 1 Orange
// 2 Banana
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.5】【强制】-  在截取切片数据时，使用[<from>:<to>]（其中， from和to都是index值，不截取结果不包含to位的数据）。

```text
【说明】
使用[from:to]方式可以避免创建额外的拷贝，降低内存开销。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th></tr></thead>
<tbody>
<tr><td>

```go
slice1 := []string{"A", "B", "C", "D", "E"}
slice2 := slice1[2:4]
```

</td>
</tr>
</tbody></table>

<br><br>

👉【规约7.6】【强制】-  检查一个切片是否为空时，使用len(s) == 0，切勿使用== nil。
<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func isEmpty(s []string) bool {
    return len(s) == 0
}
```

</td><td>

```go
func isEmpty(s []string) bool {
    return s == nil
}
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.7】【强制】-  使用make来创建空的map。
```text
【说明】
注意make返回的不是指针，而是值。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
var (
	// m1 is safe to read and write; 
	// m2 will panic on writes. 
	m1 = make(map[T1]T2, 3) 
	m2 map[T1]T2
)
```

</td><td>

```go
var ( 
	// m1 is safe to read and write;
    // m2 will panic on writes.
    m1 = map[T1]T2{}
    m2 map[T1]T2
)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.8】【强制】-  使用“map literals”来初始化 map 。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
m := map[T1]T2{
    k1: v1,
    k2: v2,
    k3: v3,
}
```

</td><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td></tr>
</tbody></table>

<br><br>

<br><br>

👉【规约7.9】【强制】- 使用make来创建切片 或 map时，当你知道需要的容量时，请务必指定容量。

```text
【说明】
不指定容量，map将被创建在堆上，增加垃圾回收的性能开销。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[string]int, 10)
```

</td><td>

```go
m := make(map[string]int)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.10】【强制】-  在对 map 中元素进行值更新时，应通过“中间人”来实现。

```text
【说明】
这里，所谓“中间人”其实指的就是临时变量。不引入“中间人”将无法直接对 map 中结构体的 field 值进行修改。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
type Hero struct{
    age int
}

heros := make(map[string]Hero, 2)
heros["哪吒"] = Hero{ age: 8 }

t := heros["哪吒"] // 引入临时变量'hero'
t.age = 10
heros["哪吒"] = t // 覆盖

fmt.Println(heros["哪吒"].age) // 10
```

</td><td>

```go
type Hero struct{
    age int
}

heros := make(map[string]Hero, 2)
heros["哪吒"] = Hero{age: 8 }

// 直接修改结构体的field值，会出现编译错误。
heros["哪吒"].age = 10

fmt.Println(heros["哪吒"].age)
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.11】【推荐】- 在对切片进行append操作时，可以使用“3-index-slice”方式来避免对已占用的地址所存放的值进行误修改。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
slice1 := []string{"A", "B", "C", "D", "E"}
slice2 := slice1[2:4:4]
inspectSlice(slice1)
inspectSlice(slice2)

// Output:
// Length[5] Capacity[5]
// [0] 0xc00007e000 A
// [1] 0xc00007e010 B
// [2] 0xc00007e020 C
// [3] 0xc00007e030 D
// [4] 0xc00007e040 E
// Length[2] Capacity[2]
// [0] 0xc00007e020 C
// [1] 0xc00007e030 D
```

【范例说明】
该例子中使用了“3-index-slice”方式 - `[a:b:c]` - 来进行切片。其中`b=c`。这种情况下：`[a-b]` 规定了切片的长度；而`[a-c]`规定了切片的容量。这就让长度和容量相等，由此避免了值的误修改。

</td><td>

```go
slice1 := []string{"A", "B", "C", "D", "E"}
slice2 := slice1[2:4]
slice2 = append(slice2, "CHANGED")
inspectSlice(slice1)
inspectSlice(slice2)

// Output:
// Length[5] Capacity[5]
// [0] 0xc00007e000 A
// [1] 0xc00007e010 B
// [2] 0xc00007e020 C
// [3] 0xc00007e030 D
// [4] 0xc00007e040 CHANGED
// Length[3] Capacity[3]
// [0] 0xc00007e020 C
// [1] 0xc00007e030 D
// [2] 0xc00007e040 CHANGED

// append 函数增加了 slice2 的长度,
// 由此改变了地址[0xc00007e040]的值。
// 但这个地址在 slice1 中已经被占用，
// 这就导致了 slice1 中该地址的值也同时被更改了。
```

</td></tr>
</tbody></table>

<br><br>

👉【规约7.12】【推荐】- 在扩展切片时，使用append函数来追加元素。可以使用...将一个数组中的元素追加到另一个数组中。

```text
【说明】
- 使用append是安全的（总是返回拷贝）；
- append会尽可能地确保内存分配的连续性；
- 截止到（Go 1.17），append 函数的第一个参数不能为untyped nil。
```

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th></tr></thead>
<tbody>
<tr><td>

```go
s0 := []int{1, 2, 3}
s1 := []int{4, 5, 6}
s0 = append(s0, s1...)
fmt.Println(s0, cap(s0)) // [1, 2, 3, 4, 5, 6] 6 
```

</td>
</tr>
</tbody></table>

<br><br>

👉【规约7.13】【推荐】- 可以通过将切片值重置为nil来快速清除切片中值。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th></tr></thead>
<tbody>
<tr><td>

```go
s0 := []int{1, 2, 3}
s0 = nil
fmt.Println(s0, len(s0), cap(s0)) // [] 0 0
```

</td>
</tr>
</tbody></table>

<br><br>

👉【规约7.14】【推荐】-  在对 map 中的元素进行读写操作时，可以使用, ok来检查元素是否存在。

<br>

<table>
<thead><tr><th style="color:green;">GOOD</th><th style="color:red;">BAD</th></tr></thead>
<tbody>
<tr><td>

```go
func offset(tz string) int {
	if seconds, ok := timeZone[tz]; ok {
		return seconds
	}
    log.Println("unknown time zone:", tz)
    return 0
}
```

</td>

<td>

```go
func offset(tz string) int {
    if seconds := timeZone[tz]; seconds != nil {
        return seconds
    }
    log.Println("unknown time zone:", tz)
    return 0
}
```

</td></tr>

</tbody></table>

<br><br><br>

<hr>

<br>

## 附录 A - 不同类型值的大小（基于Go编译器 1.17版本）

<br>

| **类型** | **值大小** | **在GO官方文档中的说明**
| --- | --- | ---
| bool | 1 byte | not specified
| int8, uint8 (byte) | 1 byte | 1 byte
| int16, uint16 | 2 bytes | 2 bytes
| int32 (rune), uint32, float32 | 4 bytes | 4 bytes
| int64, uint64, float64, complex64 | 8 bytes | 8 bytes
| complex128 | 16 bytes | 16 bytes
| int, uint | 1 word | 因平台类型不同而不同,  32位平台上是4 bytes，64位平台上是8 bytes
| uintptr | 1 word | 足够存储空间
| string | 2 words | 未说明
| pointer (safe or unsafe) | 1 word | 未说明
| slice | 3 words | 未说明
| map | 1 word | 未说明
| channel | 1 word | 未说明
| function | 1 word | 未说明
| interface | 2 words | 未说明
| struct | (所有fields的大小之和) + (padding字符的长度) | 如果struct只有为空的field，其大小为0
| array | (元素大小) * (array长度) | 如果array元素都为空，则大小为0

** 说明：在下表中，一个word表示一个native word，在32位平台上是4个字节，而在64位平台上则是8个字节。**


