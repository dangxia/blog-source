title: First Step in Scala
date: 2015-11-21 15:51:16
tags: [Programming in Scala]
categories: [Scala]
---
### Define some variable
  + `val` is similar to a final in Java
  + `var` is not final

### Define some functions
![][define_fun]
+ style-1
```scala
def max(x: Int, y: Int): Int = {
  if (x > y) x
  else y
}
```
+ style-2
```scala
//infer result type
def max2(x: Int, y: Int) = {
  if (x > y) x else y
}
```
+ style-3
```scala
//function body is single line
def max3(x: Int, y: Int) = if (x > y) x else y
```

`Unit`:A result type of Unit indicates the function returns no interesting value. Scala’s Unit type is similar to Java’s void type, and in fact every void-returning method in Java is mapped to a Unit-returning method in Scala.

```scala
scala> def greet() = println("Hello, world!")
greet: ()Unit
```
<!--more-->
### Write some Scala scripts

```scala hello.scala
println("Hello, "+ args(0) +"!")
```

```ssh run
scala hello.scala world
```

+ script in linux
```bash
#!/bin/sh
exec scala "$0" "$@"
!#
println("Hello, "+ args(0) +"!")
```

### Loop with `while`;decide with `if`
```scala
var i = 0
while (i < args.length) {
  println(args(i))
  i += 1
}
```

**NOTE that Java’s ++i and i++ don’t work in Scala. To increment in Scala, you need to say either i = i + 1 or i += 1.**
**NOTE that in Scala, as in Java, you must put the boolean expression for a while or an if in parentheses**

### Iterate with `foreach` and `for`
+ imperative style
+ functional style
  ```scala functions are first class constructs
  args.foreach((arg: String) => println(arg))
  ```
  ```scala infer arg type
  args.foreach(arg => println(arg))
  ```
  ```scala If a function literal consists of one statement that takes a single argument, you need not explicitly name and specify the argument.
  args.foreach(println)
  ```
  ![][syntax_fun_literal.png]
  ```scala it really is a val: arg can’t be reassigned inside the body of the for expression
  for (arg <- args)
    println(arg)
  ```

### type inference
+ define some variable
+ function result type
+ function literal parameter type

### simple names
+ define some variable
```scala
scala> val msg:String = "Hello, world!"
msg: java.lang.String = Hello, world!
```

### tips
+ multiple lines
To enter something into the interpreter that spans multiple lines, just keep
typing after the first line. If the code you typed so far is not complete, the
interpreter will respond with a vertical bar on the next line.
+ escape
If you realize you have typed something wrong, but the interpreter is still
waiting for more input, you can escape by pressing enter twice:



[define_fun]: /img/scala/define_fun.png  "define_fun"
[syntax_fun_literal.png]: /img/scala/syntax_fun_literal.png "syntax_fun_literal.png"