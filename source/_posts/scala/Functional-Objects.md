title: Functional Objects
date: 2015-12-06 15:44:29
tags: [Programming in Scala]
categories: [Scala]
---
>**Immutable object trade-offs**
Immutable objects offer several advantages over mutable objects, and one potential disadvantage. First, immutable objects are often easier to reason about than mutable ones, because they do not have complex state spaces that change over time. Second, you can pass immutable objects around quite freely, whereas you may need to make defensive copies of mutable objects before passing them to other code. Third, there is no way for two threads concurrently accessing an immutable to corrupt its state once it has been properly constructed, because no thread can change the state of an immutable. Fourth, immutable objects make safe hash table keys. If a mutable object is mutated after it is placed into a HashSet, for example, that object may not be found the next time you look into the HashSet. 
The main disadvantage of immutable objects is that they sometimes require that a large object graph be copied where otherwise an update could be done in place. In some cases this can be awkward to express and might also cause a performance bottleneck. As a result, it is not uncommon for libraries to provide mutable alternatives to immutable classes. For example, class StringBuilder is a mutable alternative to the immutable String. We’ll give you more information on designing mutable objects in Scala in Chapter 18.