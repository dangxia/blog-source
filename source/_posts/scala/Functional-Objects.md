title: Chapter6：Functional Objects
date: 2015-12-06 15:44:29
tags: [Programming in Scala]
categories: [Scala]
---
### Constructing a Rational
```scala
class Rational(n: Int, d: Int)
```
this line of code is that if a class doesn’t have a body, you don’t need to specify empty curly braces (though you could, of course, if you wanted to).

**primary constructor**
>**Immutable object trade-offs**
Immutable objects offer several advantages over mutable objects, and one potential disadvantage. First, immutable objects are often easier to reason about than mutable ones, because they do not have complex state spaces that change over time. Second, you can pass immutable objects around quite freely, whereas you may need to make defensive copies of mutable objects before passing them to other code. Third, there is no way for two threads concurrently accessing an immutable to corrupt its state once it has been properly constructed, because no thread can change the state of an immutable. Fourth, immutable objects make safe hash table keys. If a mutable object is mutated after it is placed into a HashSet, for example, that object may not be found the next time you look into the HashSet. 
The main disadvantage of immutable objects is that they sometimes require that a large object graph be copied where otherwise an update could be done in place. In some cases this can be awkward to express and might also cause a performance bottleneck. As a result, it is not uncommon for libraries to provide mutable alternatives to immutable classes. For example, class StringBuilder is a mutable alternative to the immutable String. We’ll give you more information on designing mutable objects in Scala in Chapter 18.

>**NOTE:**
This initial Rational example highlights a difference between Java and Scala. In Java, classes have constructors, which can take parameters, whereas in Scala, classes can take parameters directly. The Scala notation is more concise—class parameters can be used directly in the body of the class; there’s no need to define fields and write assignments that copy constructor parameters into fields. This can yield substantial savings in boilerplate code, especially for small classes.

```scala
class Rational(n: Int, d: Int) {
	println("Created "+ n +"/"+ d)
}
```
The Scala compiler will compile any code you place in the class body, which isn’t part of a field or a method definition, into the primary constructor

### Reimplementing the toString method
```scala
class Rational(n: Int, d: Int) {
	override def toString = n +"/"+ d
}
```
The override modifier in front of a method definition signals that a previous method definition is overridden

### Checking preconditions
A precondition is a constraint on values passed into a method or constructor, a requirement which callers must fulfill
```scala
class Rational(n: Int, d: Int) {
	require(d != 0)
	override def toString = n +"/"+ d
}
```

### Adding fields
```scala
class Rational(n: Int, d: Int) { // This won’t compile
	require(d != 0)
	override def toString = n +"/"+ d
	def add(that: Rational): Rational = new Rational(n * that.d + that.n * d, d * that.d)
}
```
Although class parameters n and d are in scope in the code of your add method, you can only access their value on the object on which add was invoked. Thus, when you say n or d in add’s implementation, the compiler is happy to provide you with the values for these class parameters. But it won’t let you say that.n or that.d, because that does not refer to the Rational object on which add was invoked
```scala
class Rational(n: Int, d: Int) {
	require(d != 0)
	val numer: Int = n
	val denom: Int = d
	override def toString = numer +"/"+ denom
	def add(that: Rational): Rational =
		new Rational(
			numer * that.denom + that.numer * denom,
			denom * that.denom
		)
}
```

### Self references
The keyword `this` refers to the object instance on which the currently executing method was invoked, or if used in a constructor, the object instance being constructed


### Auxiliary constructors
Auxiliary constructors in Scala start with def this(...).
```scala
class Rational(n: Int, d: Int) {
	require(d != 0)
	val numer: Int = n
	val denom: Int = d
	def this(n: Int) = this(n, 1) // auxiliary constructor
...
}
```
>**The primary constructor is thus the single point of entry of a class.**
In Scala, every auxiliary constructor must invoke another constructor of the same class as its first action. In other words, the first statement in every auxiliary constructor in every Scala class will have the form “this(. . . )”. The invoked constructor is either the primary constructor (as in the Rational example), or another auxiliary constructor that comes textually before the calling constructor. The net effect of this rule is that every constructor invocation in Scala will end up eventually calling the primary constructor of the class.

>**NOTE**
If you’re familiar with Java, you may wonder why Scala’s rules for constructors are a bit more restrictive than Java’s. In Java, a constructor must either invoke another constructor of the same class, or directly invoke a constructor of the superclass, as its first action. In a Scala class, only the primary constructor can invoke a superclass constructor. The increased restriction in Scala is really a design trade-off that needed to be paid in exchange for the greater conciseness and simplicity of Scala’s constructors compared to Java’s.

### Private fields and methods
make a field or method private you simply place the private keyword in front of its definition.

### Defining operators
The first step is to replace add by the usual mathematical symbol
```scala
class Rational(n: Int, d: Int) {
	require(d != 0)
	private val g = gcd(n.abs, d.abs)
	val numer = n / g
	val denom = d / g
	def this(n: Int) = this(n, 1)
	def + (that: Rational): Rational =
		new Rational(
			numer * that.denom + that.numer * denom,
			denom * that.denom
		)
	def * (that: Rational): Rational =
	new Rational(numer * that.numer, denom * that.denom)
	override def toString = numer +"/"+ denom
	private def gcd(a: Int, b: Int): Int =
	if (b == 0) a else gcd(b, a % b)
}
```

### Identifiers in Scala
