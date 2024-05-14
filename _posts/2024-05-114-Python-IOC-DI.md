---
layout: post
title:  "Python IoC/DI"
date:   2024-05-08 16:00:00 +0800
categories: CS Python IoC DI
---

## 为什么IOC/DI在Python中没有Java那么流行?
看了一些帖子, 结合我自己的理解:
### DI的作用是什么
* 将系统的构建和使用分离,
  * 代码实现各个component
  * IoC container实现组装, wire components
* 在runtime实现compnents组装
* 一般通过额外的Java config或者xml配置IoC

### IoC/DI 是一种思想
* 和具体的语言实现无关, 静态的构建系统components, 动态的组装components使用系统

### Python中Ioc/DI
* Python可以实现framework类似Java中Spring独立的IoC/DI container, 将系统构建使用相分离
* 因为Python自身是动态语言, 同时也具备scripting能力, 所以完全可以不需要额外写config/xml. 相当于把Ioc/DI能力和语言本身揉在了一起, 即没有分开处理系统的构建和使用.
* 在Java中如果在一个class中new 另一个class, 那么就是紧耦合, 代码写好之后没法改变, 在Python中new 一个class完全可以dynamic runtime替换, 例如:
```python
class Dog:
    def say_hello(self):
        print("Dog says Hello.")


class Cat:
    def say_hello(self):
        print("Cat says Hello.")


def hello():
    dog = Dog()
    dog.say_hello()


hello()
# Dog says Hello.

Dog = Cat
hello()
# Cat says Hello.
```

### 总结
Python提供了更加灵活的构建系统和使用系统的方式:
* 如果Java程序员习惯Java的DI方式那么可以考虑使用Python的DI framework,
* 利用Python的动态性, 不用DI framework也可以灵活的构建和使用系统, 只不过不像Java DI分离的那么清晰

### 参考
* https://stackoverflow.com/questions/2461702/why-is-ioc-di-not-common-in-python
* https://stackoverflow.com/questions/2273683/why-are-ioc-containers-unnecessary-with-dynamic-languages
* https://stackoverflow.com/questions/31678827/what-is-a-pythonic-way-for-dependency-injection