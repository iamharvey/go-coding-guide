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

- [1. 介绍](https://github.com/iamharvey/go-coding-guide/blob/main/README.md#1-%E4%BB%8B%E7%BB%8D)
  - [1.1 阅读引导]()
  - [1.2 名词对照表]()
  - [1.3 文档组织]()
- [2. 编程规约]()
  - [2.1 命名规则（Naming）]()
  - [2.2 变量与常量（Variables and Constants）]()


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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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

| **中文名称** | **原生英文名称** |
| --- | --- |
| 包 | package
|（包）导入 | import
|（包）别名 | alias
| 类型 | type
| 接口 | interface
| 结构体 | struct
| 值 | value
| 指针 | pointer
| 接收者（方法中的概念） | receiver
| 值类接受者 | value receiver
| 指针类接受者 | pointer receiver
| 申明 | declaration
|（变量）引用 | reference
| 字符串 | string
| 字节 | byte
| 数组 | array
| 切片 | slice
| 上下文 | context
| 崩溃 | panic / panicing
| 堆 | heap
| 栈 | stack
| 垃圾回收 | garbage collection (gc)
| 返回值 | returned value(s)
| 并发 | concurrency
| 互斥锁 | mutex lock
| 原子操作 | atomic operation
| 性能测试 | Benchmark testing


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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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

👉【规约2.1】【强制】-  避免使用可被修改的全局变量。

```
【说明】
这类全局变量的问题就在于：它会在你不知情的情况下，被改变。
````

<br>

<table>
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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
<thead><tr><th>GOOD</th><th>BAD</th></tr></thead>
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

<hr>

<br>

## 附录 A - 不同类型值的大小（基于Go编译器 1.17版本）

<br>

| **类型** | **值大小** | **在GO官方文档中的说明**
| -- | -- | --
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


