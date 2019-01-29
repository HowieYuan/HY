---
layout:     post                    # 使用的布局（不需要改）
title:      经典的 Top K 问题，你真的懂了么？              # 标题 
subtitle:   #副标题
date:       2019-01-28              # 时间
author:     Howie                      # 作者
header-img: img/shiro springboot.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 算法

---

> 什么是 Top K 问题？简单来说就是在一堆数据里面找到前 K 大（当然也可以是前 K 小）的数。
>
> 这个问题也是十分经典的算法问题，不论是面试中还是实际开发中，都非常典型。而这个问题其实也有很多种做法，你真的都懂了么？

# 一. 立刻就能想到的解法
既然是要前 K 大的数，那么最直接的当然就是排序了，通过如快排等效率较高的排序算法，可以在平均 O（nlogn）的时间复杂度找到结果。

这种方式在数据量不大的时候简单可行，但固然不是最优的方法。

# 二. O(n) 时间复杂度的方法
刚刚提到了快排，熟悉算法题的小伙伴应该知道，快排的 partition 划分思想可以用于计算某个位置的数值等问题，例如用来计算中位数；显然，也适用于计算 TopK 问题

![](https://upload-images.jianshu.io/upload_images/8807674-3a9669bd3d8c7feb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每次经过划分，如果中间值等于 K ，那么其左边的数就是 Top K 的数据；
当然，如果不等于，只要递归处理左边或者右边的数即可

该方法的时间复杂度是 O(n) ，简单分析就是第一次划分时遍历数组需要花费 n，而往后每一次都折半（当然不是准确地折半），粗略地计算就是 n + n/2 + n/4 +... < 2n，因此显然时间复杂度是 O(n) 

对比第一个方法显然快了不少，随着数据量的增大，两个方法的时间差距会越来越大

## 缺点
虽然时间复杂度是 O(n) ，但是缺点也很明显，最主要的就是内存问题，在海量数据的情况下，我们很有可能没办法一次性将数据全部加载入内存，这个时候这个方法就无法完成使命了

还有一点就是这种思路需要我们修改输入的数组，这也是值得考虑的一点

# 三. 利用分布式思想处理海量数据
面对海量数据，我们就可以放分布式的方向去思考了

我们可以将数据分散在多台机器中，然后每台机器并行计算各自的 TopK 数据，最后汇总，再计算得到最终的 TopK 数据

这种数据分片的分布式思想在面试中非常值得一提，在实际项目中也十分常见

# 四. 利用最经典的方法，一台机器也能处理海量数据
其实提到 Top K 问题，最经典的解法还是利用堆。

维护一个大小为 K 的小顶堆，依次将数据放入堆中，当堆的大小满了的时候，只需要将堆顶元素与下一个数比较：如果大于堆顶元素，则将当前的堆顶元素抛弃，并将该元素插入堆中。遍历完全部数据，Top K 的元素也自然都在堆里面了。

当然，如果是求前 K 个最小的数，只需要改为大顶堆即可

![将数据插入堆](https://upload-images.jianshu.io/upload_images/8807674-d32bc53ef9cadc8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![95 大于 20，进行替换](https://upload-images.jianshu.io/upload_images/8807674-e67fb606d0f5766a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![95 下沉，维持小顶堆](https://upload-images.jianshu.io/upload_images/8807674-15eac9a1c7fb75e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于海量数据，我们不需要一次性将全部数据取出来，可以一次只取一部分，因为我们只需要将数据一个个拿来与堆顶比较。

另外还有一个优势就是对于动态数组，我们可以一直都维护一个 K 大小的小顶堆，当有数据被添加到集合中时，我们就直接拿它与堆顶的元素对比。这样，无论任何时候需要查询当前的前 K 大数据，我们都可以里立刻返回给他。

整个操作中，遍历数组需要 O(n) 的时间复杂度，一次堆化操作需要 O(logK)，加起来就是 O(nlogK) 的复杂度，换个角度来看，如果 K 远小于 n 的话， O(nlogK) 其实就接近于 O(n) 了，甚至会更快，因此也是十分高效的。

最后，对于 Java，我们可以直接使用优先队列 PriorityQueue 来实现一个小顶堆，这里给个代码：
```
public List<Integer> solutionByHeap(int[] input, int k) {
    List<Integer> list = new ArrayList<>();
    if (k > input.length || k == 0) {
        return list;
    }
    Queue<Integer> queue = new PriorityQueue<>();
    for (int num : input) {
        if (queue.size() < k) {
            queue.add(num);
        } else if (queue.peek() < num){
            queue.poll();
            queue.add(num);
        }
    }
    while (k-- > 0) {
        list.add(queue.poll());
    }
    return list;
}
```
