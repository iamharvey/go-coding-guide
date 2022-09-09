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

### 1. 介绍

<br>

这一部分是文档的介绍。其中包含了阅读引导、名词对照，并对文档的组织结构进行了说明。

<br>

### 1.1 阅读引导

为了便于在实际工作中开展实践，文档对规约的约束力进行区分。为此，文档参考了《阿里巴巴Java开发手册》后，也将所有规约分为三个等级：
- <font color="red">【强制】</font> - 必须遵从的规约。他们是代码入库的门禁；
- <font color="orange">【推荐】</font> - 推荐的规约。遵循这些规约能编写出更好、更优雅的代码，但不作为入库门禁；
- <font color="green">【参考】</font> - 可以参考的规约。是否采纳需要结合实际情况而定，也不作为入库门禁。

为了便于记忆，文档尽可能让规约的描述短小精悍。为此，文档通过添加 <span style="color:blue">【说明】</span> 部分来展现对规约的补充说明，如下所示：


