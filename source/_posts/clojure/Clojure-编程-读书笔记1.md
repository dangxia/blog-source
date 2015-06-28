title: Clojure 编程-读书笔记1
date: 2015-06-27 14:01:17
tags: [读书笔记]
category : clojure
---
## 进入Clojure仙境
### 为什么选择Clojure?
+ Clojure是运行在JVM上的一种语言
	Clojure代码可以使用任何java类库，反之Clojure库也可以被任何Java代码使用
	`？`和原生的java类库的差别?
+ Clojure是Lisp
+ Clojure是函数式编程语言
	Clojure鼓励使用高阶函数，并且提供了一些高效、不可变的数据结构，避免由于状态的不加控制的修改而导致的一些bug，并且也使Clojure成为一种进行并行、并发编程的完美语言。
	`？`java并发编程是需要状态控制的，而clojure可以直接调用java代码？
+ clojure提供一种进行并行、并发编程的创新式解决方案
	clojure的引用类型强制我们把对象的状态和对象的标识区分开。`?`
	clojure对于多线程编程的支持使得我们不用手动加锁、解锁也能编写多线程的代码。
+ clojure是一种动态的编程语言
	clojure是动态的同时也是强类型的（和Python、Ruby类似）
	clojure支持运行时更新现有代码。`？`
<!--more-->

### 命名空间
命名空间是Clojure最基本的代码模块组件，所有的Clojure代码都是在一个命名空间中被定义和求值的。
命名空间可以看成Ruby和Python中的module，Java中package。
java.lang包里面的类默认被引入到每一个Clojure命名空间中，所以可以不加包名直接访问这些Java类。
### 字面量
#### 标量字面量
+ 字符串
	Clojure中的字符串就是Java的字符串，左右两边以双引号括起来。Clojure的字符串天然支持多行。
+ 布尔值
	true和false表示布尔值。
+ 字符
	Clojure中的字符字面量是通过反斜杠加字符来表示。ex：\c
	对于Unicode编码和octal编码对应\u00ff,\o41
	特殊字符：\space,\newline,\formfeed,\return
+ nil
	对应Java中的null
+ 关键字(key)
	key始终以冒号开头,并且不和任何命名空间限定，在任何命名空间中都可以使用，效果一样。但当key里面包含的/，表示这个key是某个命名空间限定的。
	如果key以两个冒号（：：）开头，表示是当前命名空间的关键字，若同时当key里面包含的/，表示这个key是某个命名空间限定的。
	虽然书里面没有提及，但是测试`（= :user/location ::user/location)`结果为true
	
	key求值成他们本身，因为key本身就是函数，他的作用就是查找他对应的值。

{% codeblock %}
(ns user)
(def pizza {:name "Ramunto's" :location "Claremont,NH" ::location "43.3734,-72.3365" ::foo/location "test"})
(= :user/location ::user/location)
(= :user/location ::foo/location)
(:user/location pizza)
(::user/location pizza)
(:location pizza)
(:foo/location pizza)
(::foo/location pizza)
{% endcodeblock %}

### Clojure REPL
+ Read
	代码被作为字符串从输入源读入。
+ Eval
	代码被求值，产生一个结果。
+ Print
	求值的结果被打印到某个输出设备。
+ Loop
	控制重新跳回到读入(Read)阶段。

		clojure从来没有被解释执行过。
		我的理解是：repl的输入被编译成JVM的字节码，然后被动态加载而执行。

### Clojure表达式
	所有的clojure的代码都是由表达式组成的，每一个表达式会求值产生一个值。这跟其他很多语言依赖于大量无值控制语句不一样，比如跟if、for以及continue来命令式的控制程序流程不一样，clojure中的这些控制性语句都是有值的表达式，跟其他普通的表达式没有本质区别。
60 就是一个数字60的表达式，“james"就是一个字符串的表达式。
`拗口吧,反正clojure的代码全是表达式，都能被求值！`
### 代码即数据（同像性）
clojure自身的数据结构：原子值（字符串、数字等）和集合的字面量`不理解？？`。
clojure的代码是直接用表示抽象语法树（AST）的clojure的数据结构来写的。这种特征学名叫做`同像性`，一般称为`代码即数据`

