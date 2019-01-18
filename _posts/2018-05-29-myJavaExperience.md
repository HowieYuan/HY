---
layout:     post                    # 使用的布局（不需要改）
title:      分享一些 Java 后端的个人干货              # 标题 
subtitle:   #副标题
date:       2018-05-29              # 时间
author:     Howie                      # 作者
header-img: img/myJavaExperience.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Java

---
> 学习 Java 也有了不少时间，入 Java 后台的坑也有了一段时日。这段时间里，听过许多前辈的经验与分享，也看过许多大佬的文章和作品。找了个时间整理和总结了一下我个人到目前为止一路以来的听到看到或者自己感悟到的干货。
>
>这篇文章可能更多的是面对初学者，当然，只要有人看了我的这篇文章后能有所收获，我就心满意足了。

##### 本文主要有那么几个部分：

- 学习路线
- 推荐一些书籍和公众号
- 推荐一些网站，工具和资料
---

# 一. 学习路线
对于想往 java 后台这个方向发展的同学，可以按照以下路线入门：
![](https://upload-images.jianshu.io/upload_images/8807674-50042fd03449369e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 先把 java 基础部分学完，重点关注多态继承等面向对象的概念，异常，IO，容器等，这部分建议通过视频来学习（毕向东，马士兵老师等的视频都是很经典的，虽然可能年代稍久远，但是万变不离其宗）。有些人可能会推荐看书来学习（如《head first java》等），但是个人认为，如果对于初学者，看书可能相对来说比较难理解，也很容易出现看不下去的情况，而许多优秀的教学视频更加生动，也更加容易理解。
- 然后就可以开始 javaweb 的学习了，Jsp，Servlet 相对来说比较老旧，但是对于初学者理解 HTTP 请求，理解前后端的交互，理解 MVC 模式，以及对以后框架的学习还是非常有帮助的；除此之外，这个时候还需要掌握数据库的知识，以及一些基础的前端知识。
- 等到以上都搞定之后，你就可以开始你的第一个 javaweb 项目了，可以尝试完成一个具有登陆注册，增删查改等基本功能的小项目，融入自己的想法，或者在网上或者 github 上也可以找到许多练手的例子。
- 以上只是 javaweb 的基础部分，接下来你还要完成框架的学习，spring + springMVC + mybatis 是现今比较流行的框架组合，到了这部分，就要好好花多点时间来钻研学习了，框架的学习同样建议看视频来先过一遍，初步熟悉了解，再通过代码实践，项目实战以及书本来加强巩固。

关于后面的进阶，网上也有许多 “ Java 技能图谱 ”  “ Java 学习路线 ” ，这里给大家推荐两个地址参考：
 [Interview-Notebook](https://github.com/CyC2018/Interview-Notebook) 
[Java 工程师成神之路](https://mp.weixin.qq.com/s?__biz=MzI4NDY5Mjc1Mg==&mid=2247484594&idx=1&sn=2a2c26e825f9f59af18735e927469337&chksm=ebf6dccddc8155db9510c4533a7f7df13555cbfdb86c681f5f1da928b4c4bb07f40407263697&mpshare=1&scene=24&srcid=0413R9kZQE5QdYn5NJSw5LnY#rd)


# 二. 书籍
网上关于 Java 的书单很多，这里给大家推荐几本

书名 | 介绍
---|---
《JSP & Servlet 学习笔记》 |非常适合入门 JSP Servlet 的一本书，通俗易懂，书里写的一个微博小项目也很适合在学习的时候跟着一起做
《Java核心技术·卷 I》 | 比较适合入门的一本经典 java 书籍，相比 java 编程思想等更浅显易懂，也可以作为工具书使用
《Spring 实战》 | spring 的一本经典书籍，讲的十分全面，十分值得一看
《算法》 | 简单明了的书名，java 实现的算法书，比起《算法导论》更适合入门，自然也十分易懂
《Head First 设计模式》 | 非常有趣的一本书，可以把每一个设计模式讲得十分生动
《图解 HTTP》 | 让你了解程序员都必须懂的 HTTP
《深入理解 Java 虚拟机（第2版）》 | 进阶书籍，提升Java功力之良药，当然也比较难懂
《高性能 MySQL》 | MySQL的中高级教程，所以 MySQL 入门就不要看这个啦
《Java并发编程实战》 | 看“并发”俩字就知道这是一本 Java 进阶书籍
《Java 8 实战》 | Java 8 的新增功能是 Java 发生的一次很大的变化，这本书也全面讲解了 java 8 的新特性






# 三. 公众号
没事就看看，利用碎片时间，比刷微博，刷知乎好啊
### 码农翻身
![](https://upload-images.jianshu.io/upload_images/8807674-9230b582ed0c24bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
作者刘欣，擅长用讲故事的方式来讲解知识点，经常分享好玩有趣的编程知识和职场的经验教训。

### 纯洁的微笑
![](https://upload-images.jianshu.io/upload_images/8807674-08be0d34e70a737a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
总结了很多关于 spirngboot，springcloud，JVM 等的知识，还有一系列杂谈， 作者 github 的 Spring Boot 示例代码项目拥有 4K star，可以去关注看看

### Java 团长
![](https://upload-images.jianshu.io/upload_images/8807674-3d084bdb1b008df3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
经常会出一些总结性的文章，知识面很广，一个适合用来补充知识点的公众号

### 程序员小灰
![](https://upload-images.jianshu.io/upload_images/8807674-70df2b1aa250d0a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
用漫画的方式来讲解技术问题，看漫画，谁都喜欢吧~

# 四. 关于知识星球（原小密圈）
相信关注公众号多的人都知道这个，许多公众号作者都有在维护自己的知识星球。怎么说呢，知识星球本质上是一个付费论坛、社区，作为知识付费观念的一个体现，最重要的是要让自己觉得从中的获得的信息值得自己所付出的金钱，很多人花了 200 块，但是一段时间后觉得自己没什么收获，那自然是不值的。不过加入知识星球之前，首先也要做好获取信息的准备，很多人花了钱，看了一两眼，就将 APP 丢到了角落，又跑去刷微博了，那也自然是毫无用处的。收获自然是会有的，但是有多少就看每个人挖掘信息的能力了。
**当然，什么东西涉及到付费，都要留个心眼，加入之前要仔细斟酌，不要被坑了。**

# 五. 程序员的好帮手
分享一些有用的网站，工具~
###  1. Google Chrome
谷歌浏览器，不用多说了吧，程序员必备浏览器（或者还有 Firefox ？），响应快速，还有非常多好用的插件，这里先给大家推荐几个插件：

##### ①  Octotree
github 必备插件，没有之一，直接在Chrome侧边栏像打开文件夹一样的查看别人的项目，大大提高了效率

![](https://upload-images.jianshu.io/upload_images/8807674-43597eab90e25d7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

同类型的插件还有 GitCodeTree ，用于码云

##### ② JSONView
对接 api 接口的时候，返回 json 格式在原本的浏览器中的显示是杂乱无章的，利用这个接口，可以帮助你将 json 排版整理好

![](https://upload-images.jianshu.io/upload_images/8807674-2e75c900cecf9afe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### ③ FireShot
网页截图工具，支持滚动截屏，还可以直接保存为PDF，JPEG，GIF，PNG或BMP，功能众多

##### ④ 划词翻译
翻译插件很多，这个是个人认为比较好用的一个划词翻译插件，响应快速，是提高效率的一个好工具

##### ⑤postman
准确地来说，这是一个 Chrome 应用，它是一个发送请求的利器，用来调试 api，调试网络最好不过。

这里再给大家推荐一篇文章：[吐血推荐珍藏的 Chrome 插件](https://mp.weixin.qq.com/s?__biz=MzA4NTQwNDcyMA==&mid=402064553&idx=1&sn=4bc95ed03916f87cc8dfd17baed54f24&mpshare=1&scene=1&srcid=0528Gk2SDCTMD3KRyqA5ioGm#rd)

###  2. IntelliJ IDEA
很多人都在劝戒说尽早放弃 eclipse 来投入 IDEA 的怀抱，IDEA 的确是非常的好用，不过我在这里也只是推荐，使用什么还是看个人喜欢。
IDEA 里面有非常多好用的插件，这里先给大家几个：

插件 | 介绍
---|---
CamelCase  | 变量命名类型快速转换，比如驼峰命名，下划线命名
CodeGlance  |右侧增加一块代码小地图
Translation |翻译插件
Alibaba Java Coding Guidelines|《阿里巴巴Java开发规约》扫描插件，让你的编码规范起来



###  3. [GitHub](https://github.com/)
程序员交友网站，作为全世界最大的开源社区，GitHub 可以说是程序员的必备网站，学会使用 GitHub 也是程序员的必学科目。
这里给大家推荐一份 github 教程： [learn-github-from-zero](https://pan.baidu.com/s/1gfn3U2v)，作者 stormzhang

###  4. [掘金](https://juejin.im/timeline)
一个高质量的技术社区，一方面也很喜欢掘金网页的 UI，现在也越来越多人加入进来。

###  5. [LeetCode](https://leetcode.com/)
简单来说就是一个刷题网站，里面有各种类型的算法题目，每天做那么一点，既能加强算法，也能保持编码手感以及训练写白板代码。
最近也是出了 leetcode 中国的网站，不过个人推荐还是使用国外版，毕竟人多，讨论什么的也多。

###  6. [七牛云](https://portal.qiniu.com/)
一个方便快捷的云存储平台，可以用作些小项目的云存储，同时它为所有用户提供了免费的额度，是一个很适合个人使用的云存储平台。

### 7. [ProcessOn](https://www.processon.com/)
一个很好用的在线作图网站，支持流程图、思维导图、原型图、UML、网络拓扑图等，不过呢，免费版每个账号只能新建 9 个文件，当然，如果不想花这个钱的话，作完一张图之后保存到本地，然后重新作图也是可以的。

### 8. Snipaste
一个方便快捷的截屏软件，个人非常喜欢

### 9. 有道云笔记
平日里的学习记笔记是很重要的。为知，有道，印象等笔记软件都有用过，为什么推荐有道呢，一方面免费，一方面界面好看，一方面 markdown 渲染相对更好，除此之外还能够进行分享。不过有个缺点就是 markdown 编辑器的卡顿问题。
当然如果有更好的笔记软件也希望大家分享。

### 10. [阿里巴巴 Java 手册](http://techforum-img.cn-hangzhou.oss-pub.aliyun-inc.com/1528269849853/Java_manual.pdf)
阿里官方 Java 代码规范标准，涵盖了编程规约、单元测试规约、异常日志规约、MySQL规约、工程规约、安全规约等。经历过几次的迭代更新，目前更新为 1.4 详尽版，之后又公布了该规约的插件（上文有提到），可以说是真正的干货了。

---

以上就是我的个人分享，目前暂时想到那么多，以后有新的东西再更新，希望能给大家带来帮助！










