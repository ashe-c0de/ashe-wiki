## 为什么需要单例模式

假设一个办公室的所有手机都有连接WiFi的需求，那么并不需要为每部手机提供一个WiFi，而是一个WiFi供所有手机使用，那么这个WiFi就是单例模式。在编程领域，单例模式设计大大节省了内存资源！

## Definition

[Singleton Pattern](https://en.wikipedia.org/wiki/Singleton_pattern)

## 如何保证单例

在需要手动实现单例模式的时候，一般推荐Double Check Locking来实现懒加载（Lazy Loading）单例。

1.check，instance不为null，return instance

2.instance为null，try lock

3.check, instance不为null，return instance；instance为null，创建instance

同时，也可以通过一些编程语言提供的特性来实现单例。

