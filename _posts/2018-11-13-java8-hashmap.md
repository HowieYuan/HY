---
layout:     post                    # 使用的布局（不需要改）
title:      Java 程序员都该懂的 Java8 HashMap               # 标题 
subtitle:    #副标题
date:       2018-11-13              # 时间
author:     Howie                      # 作者
header-img: img/java8-hashmap.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Java

---

> HashMap 一直是非常常用的数据结构，也是面试中十分常问到的集合类型，今天就来说说 HashMap。
>
> 但是为什么要专门说明是 Java8 的 HashMap 呢？我们都知道，Java8 有很多大的变化和改动，如函数式编程等，而 HashMap 也有了一个比较大的变化。

# 先了解一下 Map
![](https://upload-images.jianshu.io/upload_images/8807674-349d401a830c0622.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


常见的Map类型有以下几种：
##### HashMap：
  - 无序
  - 访问速度快
  - key不允许重复（只允许存在一个null key）
##### LinkedHashMap：
  - 有序
  - HashMap 子类
##### TreeMap：
  - TreeMap 中保存的记录会根据 Key 排序（默认为升序排序），因此使用 Iterator 遍历时得到的记录是排过序的
  - 因为需要排序，所以TreeMap 中的 key 必须实现 Comparable 接口，否则会报 ClassCastException 异常
  - TreeMap 会按照其 key 的 compareTo 方法来判断 key 是否重复

除了上面几种以外，我们还可能看到过一个叫 Hashtable 的类：
##### Hashtable：
  - 一个遗留类，线程安全，与 HashMap 类似
  - 当不需要线程安全时，选择 HashMap 代替
  - 当需要线程安全时，使用 ConcurrentHashMap 代替


# HashMap
我们现在来正式看一下 HashMap

首先先了解一下 HashMap 内部的一些主要特点：
- 使用哈希表（散列表）来进行数据存储，并使用链地址法来解决冲突
- 当链表长度大于等于 8 时，将链表转换为红黑树来存储
- 每次进行二次幂的扩容，即扩容为原容量的两倍

## 字段
HashMap 有以下几个字段：
- Node[] table：存储数据的哈希表；初始长度 length = 16（```DEFAULT_INITIAL_CAPACITY```），扩容时容量为原先的两倍（n * 2）
- final float loadFactor：负载因子，确定数组长度与当前所能存储的键值对最大值的关系；不建议轻易修改，除非情况特殊
- int threshold：所能容纳的 key-value 对极限 ；threshold = length * Load factor，当存在的键值对大于该值，则进行扩容
- int modCount：HashMap 结构修改次数（例如每次 put 新值使则自增 1）
- int size：当前 key-value 个数

## 方法
### hash(Object key)
我们都知道，Object 类的 hashCode 方法与 HashMap 息息相关，因为 HashMap 便是通过 hashCode 来确定一个 key 在数组中的存储位置。（这里大家都应该了解一下 hashCode 与 equals 方法之间的关系与约定，这里就不多说了）

但值得注意的是，HashMap 并非直接使用 hashCode 作为哈希值，而是通过这里的 hash 方法对 hashCode 进行一系列的移位和异或处理，这样处理的目的是为了有效地避免哈希碰撞

当然，Java 8 之前的做法和现在的有所不同，Java 8 对此进行了改进，优化了该算法
```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

### put(K key, V value)
put 方法是 HashMap 里面一个十分核心的方法，关系到了 HashMap 对数据的存储问题。
```
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```
put 方法直接调用了 putVal 方法，这里我为大家加上了注释，可以配合下面的流程图一步步感受：
```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    HashMap.Node<K, V>[] tab;
    HashMap.Node<K, V> p;
    int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        //初始化哈希表
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        //通过哈希值找到对应的位置，如果该位置还没有元素存在，直接插入
        tab[i] = newNode(hash, key, value, null);
    else {
        HashMap.Node<K, V> e;
        K k;
        //如果该位置的元素的 key 与之相等，则直接到后面重新赋值
        if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof HashMap.TreeNode)
            //如果当前节点为树节点，则将元素插入红黑树中
            e = ((HashMap.TreeNode<K, V>) p).putTreeVal(this, tab, hash, key, value);
        else {
            //否则一步步遍历链表
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    //插入元素到链尾
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1)
                        //元素个数大于等于 8，改造为红黑树
                        treeifyBin(tab, hash);
                    break;
                }
                //如果该位置的元素的 key 与之相等，则重新赋值
                if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        //前面当哈希表中存在当前key时对e进行了赋值，这里统一对该key重新赋值更新
        if (e != null) { 
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    //检查是否超出 threshold 限制，是则进行扩容
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```
主要的逻辑步骤在此：

![](https://upload-images.jianshu.io/upload_images/8807674-81a9cb47f00525ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有个值得注意的有趣的地方：在 Java 8 之前，HashMap 插入数据时一直是插入到链表表头；而到了 Java 8 之后，则改为了尾部插入。至于头插入有什么缺点，其中一个就是在并发的情况下因为插入而进行扩容时可能会出现链表环而发生死循环；当然，HashMap 设计出来本身就不是用于并发的情况的。

#### （1）懒加载
我们在 HashMap 的构造函数中可以发现，哈希表 Node[] table 并没有在一开始就完成初始化；观察 put 方法可以发现：
```
if ((tab = table) == null || (n = tab.length) == 0)
      n = (tab = resize()).length;
```
当发现哈希表为空或者长度为 0 时，会使用 resize 方法进行初始化，这里很显然运用了 lazy-load 原则，当哈希表被首次使用时，才进行初始化

#### （2）树化
Java8 中，HashMap 最大的变动就是增加了树化处理，当链表中元素大于等于 8，这时有可能将链表改造为红黑树的数据结构，为什么我这里说可能呢?
```
final void treeifyBin(HashMap.Node<K,V>[] tab, int hash) {
    int n, index; HashMap.Node<K,V> e;
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        //......
}
```
我们可以观察树化处理的方法 treeifyBin，发现当```tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY```为 true 时，只会进行扩容处理，而没有进行树化；MIN_TREEIFY_CAPACITY 规定了 HashMap 可以树化的最小表容量为 64，这是为了避免在哈希表建立初期，多个键值对恰好被放入了同一个链表中而导致不必要的转化。

那么，HashMap 为什么要进行树化呢？我们都知道，链表的查询效率大大低于数组，而当过多的元素连成链表，会大大降低查询存取的性能；同时，这也涉及到了一个安全问题，一些代码可以利用能够造成哈希冲突的数据对系统进行攻击，这会导致服务端 CPU 被大量占用。

### resize()
扩容方法同样是 HashMap 中十分核心的方法，同时也是比较耗性能的操作。

我们都知道数组是无法自动扩容的，所以我们需要重新计算新的容量，创建新的数组，并将所有元素拷贝到新数组中，并释放旧数组的数据。

与以往不同的是，Java8 规定了 HashMap 每次扩容都为之前的两倍（n*2），也正是因为如此，每个元素在数组中的新的索引位置只可能是两种情况，一种为不变，一种为原位置 + 扩容长度（即偏移值为扩容长度大小）；反观 Java8 之前，每次扩容需要重新计算每个值在数组中的索引位置，增加了性能消耗

接下来简单给大家说明一下，上一段话是什么意思：
前面讲 put 的时候我们知道每个元素在哈希表数组中的位置等于 (n - 1) & hash，其中 n 是当前数组的大小，hash 则是前面讲到的 hash 方法计算出来的哈希值

![](https://upload-images.jianshu.io/upload_images/8807674-91d8722e3873ce69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图中我们可以看到，扩容前 0001 0101 和 0000 0101 两个 hash 值最终的计算出来的数组中的位置都是 0000 0101，即为 5，此时数组大小为 0000 1111 + 1 即 16

扩容后，数组从 16 扩容为两倍即 32（0001 1111），此时原先两个 hash 值计算出来的结果分别为 0001 0101 和 0000 0101 即 21 和 5，两个数之间刚好相差 16，即数组的扩容大小

这个其实很容易理解，数组扩容为原来的两倍后，n - 1 改变为 2n - 1，即在原先的二进制的最高位发生了变化

![](https://upload-images.jianshu.io/upload_images/8807674-85bd1e0622b72337.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此进行 ＆ 运算后，出来的结果只可能是两种情况，一种是毫无影响，一种为原位置 + 扩容长度

那么源代码中是如何判断是这两种情况的哪一种呢？我们前面说到，HashMap 中数组的大小始终为 16 的倍数，因此 hash & n 和 hash & (2n - 1) 分别计算出来的值中高位是相等的

![](https://upload-images.jianshu.io/upload_images/8807674-0edd7bf8a2642f91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此源码中使用了一个非常简单的方法（oldCap 是原数组的大小，即 n）
```
if ((e.hash & oldCap) == 0) {
    ...
} else {
    ...
}
```
当 e.hash & oldCap 等于 0 时，元素位置不变，当非 0 时，位置为原位置 + 扩容长度

### get(Object key)
了解了 HashMap 的存储机制后，get 方法也很好理解了
```
final HashMap.Node<K,V> getNode(int hash, Object key) {
    HashMap.Node<K,V>[] tab; HashMap.Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 && (first = tab[(n - 1) & hash]) != null) {
        //检查当前位置的第一个元素，如果正好是该元素，则直接返回
        if (first.hash == hash && ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            //否则检查是否为树节点，则调用 getTreeNode 方法获取树节点
            if (first instanceof HashMap.TreeNode)
                return ((HashMap.TreeNode<K,V>)first).getTreeNode(hash, key);
            //遍历整个链表，寻找目标元素
            do {
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```
主要就四步：
1. 哈希表是否为空或者目标位置是否存在元素
2. 是否为第一个元素
3. 如果是树节点，寻找目标树节点
4. 如果是链表结点，遍历链表寻找目标结点
