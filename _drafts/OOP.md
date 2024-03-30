
## OO是什么? Definitions For OO?
对于OO的定义有很多不同的声音, See https://wiki.c2.com/?DefinitionsForOo.

同时也有不停的争论,  每个观点的角度不同, 停留在争论中没有意义. 还是要看主流技术开源社区代码是怎么OOP落地的, 好的OOP代码是怎么设计和实现的. 对于我来说, 看到Kai提到OOP messaging的重要性还是醍醐灌顶, 在做OO design时, 一开始跳出实现细节, 更多的从Objects之间交互去设计.

#### 1.Polymorphism, Encapsulation, Inheritance
See https://wiki.c2.com/?PolymorphismEncapsulationInheritance

这个观点似乎主要是从C++社区发展出来的, 如果一个语言支持面向对象编程(OOP), 那么就支持三大特征:
> **Polymorphism**; in this context this really means SubtypePolymorphism. As pointed out below, there are many other types.
> 
> **Encapsulation**; there are means to segregate an object's interface from its implementation. This may be enforced by the language (as in CeePlusPlus and JavaLanguage) or by convention (as in PythonLanguage and CommonLispObjectSystem)
> 
> **Inheritance**; reuse of base classes to form derived classes. Some object to this, as other mechanisms exist (interface inheritance, delegation, structural typing) to implement subtypes.

* 注意这里的Polymorphism是SubtypePolymorphism, 通过subtyping支持多态
* Inheritance会破坏Encapsulation
* 还有别的方式实现Polymorphism, ParametricPolymorphism:
    > There are other ways to achieve Polymorphism, namely ParametricPolymorphism (STL = StandardTemplateLanguage) and Generics(TheRightWayToImplementTemplates).
    > Generics are ParametricPolymorphism. While **JavaGenerics** and **CppTemplates** and **DynamicTyping** all go about it the same way; they all are ways of implementing ParametricPolymorphism

#### 2.Alan Kay's Definition of Object Oriented
Read details on https://wiki.c2.com/?AlanKaysDefinitionOfObjectOriented

> "Alan Kay, considered by some to be the father of object-oriented programming, identified the following characteristics as fundamental to OOP:"
> 1. EverythingIsAnObject.
> 2. Communication is performed by objects communicating with each other, requesting that objects perform actions. Objects communicate by sending and receiving messages. A message is a request for action, bundled with whatever objects may be necessary to complete the task.
> 3. Objects have their own memory, which consists of other objects.
> 4. Every object is an instance of a class. A class simply represents a grouping of similar objects, such as integers or lists.
> 5. The class is the repository for behavior associated with an object. That is, all objects that are instances of the same class can perform the same actions.
> So far, similar to 1-5 above. Rule 6 is different. The reference to lists is removed, instead we have:
> 6. Classes are organized into a singly-rooted tree structure, called the inheritance hierarchy. Memory and behavior associated with instances of a class are available to any class associated with a descendent in this tree structure.
> 

A later version:
> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme LateBinding of all things.
> ~ -- Alan Kai

Reference:
* Isolation, Extreme Late Binding, Messaging, [Alan Kay and OO Programming](https://ovid.github.io/articles/alan-kay-and-oo-programming.html)
* https://softwareengineering.stackexchange.com/questions/46592/so-what-did-alan-kay-really-mean-by-the-term-object-oriented/58732#58732


这一段表述很直接, 我比较认同, 不要把OO想的太复杂了, from [Alan Kay and OO Programming](https://ovid.github.io/articles/alan-kay-and-oo-programming.html)
> And that's it. Objects are simply experts. You tell them what you need and they get it done. Forget all of the handwaving about blueprints or "data with behaviors." Those are implementation details. And once you start thinking about objects as simply experts about a particular problem domain, OOP becomes much easier.

## 读[The Forgotten History of OOP](https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f) 笔记
> “I made up the term ‘object-oriented’, and I can tell you I didn’t have C++ in mind.” ~ Alan Kay, OOPSLA ‘97
> 

> “OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.”
> ~ Alan Kay

> In other words, according to Alan Kay, the essential ingredients of OOP are:
> - Message passing
> - Encapsulation
> - Dynamic binding
> 
> Notably, inheritance and subclass polymorphism were NOT considered essential ingredients of OOP by Alan Kay, the man who coined the term and brought OOP to the masses.

## 我的一些理解:
OO有不同层次定义, 有的侧重 OO Language, 有的侧重system层面.

不同的OO definition 角度不同
Kay: messaging, 侧重高层设计层面， 系统层面OO，Objects 如何交互

继承+封装+多态：更多的侧重具体实现层面， 语言实现层面, Ojbects如何用代码实现，并且支持good reusable, maintainable code

看了一些资料，理解到继承并不是OOP的核心概念。
继承更像是一种实现上的细节，这里又分两种继承
1. 接口继承，type的扩展，为多态服务
    1.在Alan Kay的OOP定义中，多态不需要完全通过subclassing来实现。比如Python中对象不需要继承同一个类，只需要定义同样的方法就可以被多态的调用。再比如，一开始Samlltalk中没有subclassing:
    
    > https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f
    > 
    > Smalltalk was developed by Alan Kay, Dan Ingalls, Adele Goldberg, and others at Xerox PARC. Smalltalk was more object-oriented than Simula — everything in Smalltalk is an object, including classes, integers, and blocks (closures). The original Smalltalk-72 did not feature subclassing. That was introduced in Smalltalk-76 by Dan Ingalls.
    > 
    > While Smalltalk supported classes and eventually subclassing, Smalltalk was not about classes or subclassing things. It was a functional language inspired by Lisp as well as Simula. Alan Kay considers the industry’s focus on subclassing to be a distraction from the true benefits of object oriented programming.
    
    
2. 实现继承，为了代码复用
(看 流畅的Python 做笔记)


注意，有时接口继承和实现继承会同时发生，比如在Java中，一个class A继承另一个class B(注意是class而不是interface)时，A同时是B的子类型，同时也继承了B中的代码；如果B同时也继承了接口interface C, 那个A也是C的子类型

From Kai:
> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things。

我理解的Alan Kay的OOP定义是一种高层abstraction, 每种号称实现OOP的语言(See [OOP languages](https://en.wikipedia.org/wiki/Object-oriented_programming#OOP_languages))都有其不同的支持OOP的实现细节。所以如果从不同语言的角度去看OOP(比如从Java/C++/Python)，那么在核心OOP概念支持之外，还会看到不同的OOP实现细节, 比如：
1. 有无interface直接定义
2. 是否支持多继承
3. late-binding 如何实现
4. 等等

写到这里，想到如果从Kai核心OOP的概念出发，很多具体语言的OOP实现细节就变得不是很重要了，比如
1. 有无interface
2. 有无多继承（Python我目前都没用过多继承）

---
OOP的具体定义也是有一些争议的，定义也比较偏理论。从原理上理解OOP对coding会有些帮助，但就像每种理论都不能脱离实践一样，**要像写好代码需要多去看业界好的开源代码是怎么写的、怎么用好OOP的**
现在的OOP, 本身就是理论+具体语言实现细节落地，不可能脱离具体语言去谈OOP（哪怕是UML, UML也是一种设计语言也定义了接口、类、继承等等）

不同的高级语言对OOP的支持也有差别

---
注意看Design Patterns 书的副标题：Elements of Reusable Object-Oriented Software
`Reusable` 这个词和《奇思妙想》P35 Kay的一段话想辉映，但是Kay是说Object level的reusable, Design Patterns中是一种比Object更高层次的抽象，上升到解决某一类问题patterns和背后设计思想的复用reusable.
// TODO
// 《奇思妙想》P35 Kay
// Design patterns words 

---
读Thinking in Java 前言、第一章，整理总结

* 每一种语言都有其控制复杂度的考虑和专长, 学习和适应新的语言是不要抱着抵触的心理, 每学会一种新语言就相当于学会一种新的控制复杂性的方法, 针对某一类问题新学会的语言也许会提供好的复杂性控制方法. 比如OO类语言, 函数式语言, 再比如Go管理处理并发, Python AI和数据处理