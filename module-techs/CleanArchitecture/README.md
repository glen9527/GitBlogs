# 《架构整洁之道》（*Clean Architecture*）

## 书评
《架构整洁之道》描述架构的方式不仅在于书的内容，还在于它整本书的结构。

其中，第 5 部分（软件架构）和第 6 部分（实现细节）感觉对目前提升作用不大。待后续重读。
## 目录
## 第 1 部分 概述
## 第 1 章 设计与架构究竟是什么
## 第 2 章 两个价值维度

## 第 2 部分 从基础构件开始：编程范式
## 第 3 章 编程范式总览
- 结构化编程
    - 结构化编程是第一个普遍被采用的编程范式（但是却不是第一个被提出来的），由 Edsger Wybe Dijkstra 于 1968 年最先提出。与此同时，Dijkstra 还论证了使用 goto 这样的无限制跳转语句将会损害程序的整体结构。
    - `结构化编程对程序控制权的直接转移进行了限制和规范。`
- 面向对象编程
    - 说到编程领域中第二个被广泛采用的编程范式，当然就是面向对象编程了。事实上，这个编程范式的提出比结构化编程还早了两年，是在 1966 年由 Ole Johan Dahl 和 Kriste Nygaard 在论文中总结归纳出来的。这两个程序员注意到在 ALGOL 语言中，函数调用堆栈（call stack frame）可以被挪到堆内存区域内，这样函数定义的本地变量就可以在函数返回之后继续存在。这个函数就成为了一个类（class）的构造函数，而它所定义的本地变量就是类的成员变量，构造函数定义的嵌套函数就成为了方法（method）。这样一来，我们就可以利用多态（polymorphism）来限制用户对函数指针的使用。
    - `面向对象编程对程序控制权的间接转移进行了限制和规范。`
- 函数式编程
    - 尽管第三个编程范式是近些年才刚刚开始被采用的，但它其实是三个范式中最先被发明的。事实上，函数式编程概念是基于阿兰·图灵同时代的数学家 Alonzo Church 在 1936 年发明的 λ 演算的直接衍生物。1958 年 John Mccarthy 利用其作为基础发明了 LISP 语言。众所周知，λ 演算法的一个核心思想是不可变性——某个符号所对应的值是永远不变的，所以从理论上来说，函数式编程语言应该是没有赋值语句的。大部分函数式编程语言只允许在非常严格的限制条件下，才可以更改某个变量的值。
    - `函数式编程对程序中的赋值进行了限制和规范。`
## 第 4 章 结构化编程
结构化编程范式中最有价值的地方就是，它赋予了我们创造可证伪程序单元的能力。这就是为什么现代编程语言一般不支持无限制的 goto 语句。更重要的是，这也是为什么在架构设计领域，`功能性降解拆分仍然是最佳实践之一`。

无论在哪一个层面上，从最小的函数到最大组件，软件开发的过程都和科学研究非常相似，它们都是由证伪驱动的。软件架构师需要定义可以方便地进行证伪（测试）的模块、组件以及服务。为了达到这个目的，他们需要将类似结构化编程的限制方法应用在更高的层面上。
## 第 5 章 面向对象编程
面向对象编程到底是什么？业界在这个问题上存在着很多不同的说法和意见。然而对一个软件架构师来说，其含义应该是非常明确的：面向对象编程就是以多态为手段来对源代码中的依赖关系进行控制的能力，这种能力让软件架构师可以构建出某种插件式架构，让高层策略组件与底层实现性组件相分离，底层组件可以被编译成插件，实现独立于高层组件的开发和部署。
## 第 6 章 函数式编程

## 第 3 部分 设计原则
> 通常来说，要想构建一个好的软件系统，应该从写整洁的代码开始做起。毕竟，如果建筑所使用的砖头质量不佳，那么架构师所起到的作用也会很有限。反之亦然，如果建筑的架构设计不佳，那么其所用的砖头质量再好也没有用。这就是 SOLID 设计原则索要解决的问题。
## 第 7 章 SRP：单一职责原则
## 第 8 章 OCP：开闭原则
## 第 9 章 LSP：里氏替换原则
## 第 10 章 ISP： 接口隔离原则
## 第 11 章 DIP：依赖反转原则

## 第 4 部分 组件构建原则
> 大型软件系统的构建过程与建筑物修建很类似，都是由一个个小组件组成的。所以，如果说 SOLID 原则用于指导我们如何将砖块砌成墙与房间的，那么组件构建原则就是用来指导我们如何将这些房间组合成房子的。
## 第 12 章 组件
> 我们常常会在程序运行时插入某些动态链接文件，这些动态链接文件所使用的就是软件架构中的组件概念。在经历了 50 年的演进后，组件化的插件 式架构已经成为我们习以为常的软件构建形式了。
## 第 13 章 组件聚合
- `REP：复用/发布等同原则`：软件复用的最小粒度应等同于其发布的最小粒度。
- `CCP：共同闭包原则`：我们应该将那些会同时修改，并且为相同目的而修改的类放到同一个组件中，而降不会同时修改，并且不会为了相同目的而修改的那些类放到不同的组件中。
- `CRP：共同复用原则`：不要强迫一个组件的用户依赖他们不需要的东西。
## 第 14 章 组件耦合

## 第 5 部分 软件架构
## 第 15 章 什么是软件架构
## 第 16 章 独立性
## 第 17 章 划分边界
## 第 18 章 边界剖析
## 第 19 章 策略与层次
## 第 20 章 业务逻辑
## 第 21 章 尖叫的软件架构
## 第 22 章 整洁架构
## 第 23 章 展示器与谦卑对象
## 第 24 章 不完全边界
## 第 25 章 层次与边界
## 第 26 章 Main 组件
Hunt the Wumpus 游戏 [github](https://github.com/gdut-yy/HuntTheWumpus-CleanArchitecture)
## 第 27 章 服务：宏观与微观
## 第 28 章 测试边界
## 第 29 章 整洁的嵌入式架构
Kent Beck 描述了软件构建过程中的三个阶段（引号部分是他的原话，楷体部分是我的注解）：

1. “先让代码工作起来” —— 如果代码不能工作，就不能产生价值。
2. “然后再试图将它变好” —— 通过对代码进行重构，让我妈自己和其他人更好地了解代码，并能按照需求不断地修改代码。
3. “最后再试着让它运行得更快” —— 按照性能提升的 “需求” 来重构代码。

我所见过的大部分 “野生” 的嵌入代码，都只关注 “先让它工作起来” 这个目标 —— 也许还有些团队会同时痴迷于 “让它更快” 这个目标，不放过任何一个机会加入各种微优化。在《人月神话》这本书中，Fred Brooks 建议我们应该随时准备“抛弃一个设计”。Kent 和 Fred 说的其实是同一件事：“在实践中学习正确的工作方法，然后再重写一个更好的版本。”

## 第 6 部分 实现细节
## 第 30 章 数据库只是实现细节
## 第 31 章 Web 是实现细节
## 第 32 章 应用程序框架是实现细节
## 第 33 章 案例分析：视频销售网站
## 第 34 章 拾遗
- 按层封装
- 按功能封装
- 端口和适配器
- 按组件封装