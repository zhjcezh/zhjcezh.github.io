---
layout:     post
title:      "Collection集合"
subtitle:   "数据结构"
date:       2019-03-19 12:00:00
author:     "Cfeng"
header-img: "img/home-bg.jpg"
catalog: true
tags:
    - 数据结构
    - 笔记
---


# Arraylist 与 LinkedList 
1. 是否保证线程安全： ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；

2. 底层数据结构： **Arraylist 底层使用的是Object数组**；LinkedList 底层使用的是双向链表数据结构（注意双向链表和双向循环链表的区别：）；
3. 插入和删除是否受元素位置的影响： ① ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行add(E e) 方法的时候， ArrayList 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是O(1)。但是如果要在指定位置 i 插入和删除元素的话（add(int index, E element) ）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ② LinkedList 采用链表存储，所以插入，删除元素时间复杂度不受元素位置的影响，都是近似 O（1）而数组为近似 O（n）。
4. 是否支持快速随机访问： LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index) 方法)。
5. 内存空间占用： ArrayList的空间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

ArrayList继承了RandomAccess
```
public interface RandomAccess {
}
```
RandomAccess标识实现这个接口的类具有随机访问功能。ArrayList实现了快速随机访问，所以它实现RandomAccess。
而LinkedList没有实现随机访问，所以没有实现
所以是**原先就实现了功能，然后靠实现RandomAccess进行标识**
通过判断是否实现RandomAccess判断遍历方式
实现了RandomAccess接口的list，优先选择普通for循环 ，其次foreach,
未实现RandomAccess接口的ist，优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大size的数据，千万不要使用普通for循环

# HashMap
JDK1.8 之前 HashMap 底层是数组和链表结合在一起使用也就是链表散列。HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的时数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

JDK1.8之后在解决哈希冲突时有了较大的变化，当**链表长度大于阈值（默认为8）时**，将链表转化为**红黑树**，以减少搜索时间。

## 红黑树
* 每个节点非红即黑；
* 根节点总是黑色的；
* 每个叶子节点都是黑色的空节点（NIL节点）；
* 如果节点是红色的，则它的子节点必须是黑色的（反之不一定）；
* 从根节点到叶节点或空子节点的每条路径，必须包含相同数目的黑色节点（即相同的黑色高度）

红黑树就是为了解决二叉查找树的缺陷，因为二叉查找树在某些情况下会退化成一个线性结构
红黑树在插入新数据后可能需要通过左旋，右旋、变色这些操作来保持平衡。所以选择8

## hashmap与hashtable
1. 线程是否安全： HashMap 是非线程安全的，HashTable 是线程安全的；HashTable 内部的方法基本都经过 synchronized 修饰。（如果你要保证线程安全的话就使用**ConcurrentHashMap**吧！）；

2. 效率： 因为线程安全的问题，HashMap要比HashTable效率高一点。另外，*HashTable基本被淘汰*，不要在代码中使用它；
3. 对Null key 和Null value的支持： HashMap 中，null 可以作为键，这样的**键只有一个**，可以有**一个或多个键所对应的值**为 null。。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 NullPointerException。
4. 初始容量大小和每次扩充容量大小的不同 ： ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的**2n+1**。HashMap 默认的初始化大小为16。之后每次扩充，**容量变为原来的2倍**。②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其**扩充为2的幂次方大小**（HashMap 中的tableSizeFor()方法保证，下面给出了源代码）。也就是说 HashMap 总是使用2的幂作为哈希表的大小,后面会介绍到为什么是2的幂次方。
5. 底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

# ConcurrentHashMap 与 HashTable
实现线程安全的方式（重要）： 
① 在JDK1.7的时候，ConcurrentHashMap（分段锁） 对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。（默认分配16个Segment，比Hashtable效率提高16倍。） 到了 JDK1.8 的时候已经摒弃了Segment的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 synchronized 和 CAS 来操作。（JDK1.6以后 对 synchronized锁做了很多优化） 整个看起来就像是优化过且线程安全的 HashMap，虽然在JDK1.8中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
② Hashtable(同一把锁) :使用synchronized来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。

# LinkedHashMap
LinkedHashMap简单来说是一个有序的HashMap，其是HashMap的子类，HashMap是无序的。
LinkedHashMap提供的节点添加了两个属性before和after节点，用来生成双向循环列表的，这样每次在Map中添加值都会追加到链表的最后一位，这样就按照插入顺序生成了一个链表



1
1
1
1
1
1
1
1
1
1
1
1
1
1
1
1
1