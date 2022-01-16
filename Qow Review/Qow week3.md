# Week3：Semi Structured Data

`paradigm`范例

XML的query 是怎么进行的

NoSQL database 知识（支持SSD）

# 主要内容

1. XML
2. NoSQL

# 考点

1. XML path rewriting 优化
2. XPath语句
3. 画pre/post encoding plot
4. NoSQL Features
5. Distributed Hash Table 那个环

## XPath rewriting optimization

> 宗旨：避免遍历所有节点，只遍历需要的部分：把path变distinct

方法：把能确定的部分都用确定的路径写

![image-20220114181858401](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141818542.png)

前提：XML的结构是知道的，所以知道哪个节点是必经之路

> path summary tree 就是把可以合并的节点合并掉

### optimization 在 XQuery 中的表现

从能计算的部分先开始，然后自上往下

![image-20220114182206897](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141822042.png)

## 画pre/post encoding plot

涉及到了 XML 的 Encoding 机制

用一个图：`Axis`表示与当前节点的关系

![image-20220114182805979](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141828084.png)

前序遍历做横轴，后序遍历做纵轴

> 规律总结：以某节点为原点画十字
>
> 左上祖先 ancester，右下子孙 descendant
>
> 左下前驱 preceding，右上后继 following

**Axes level**

父节点parent level = 当前节点level -1

子节点children = 当前节点leve + 1

following-sibling 和 preceding-sibling 与当前节点level相同

## NoSQL

### 关于几个问题

1. 什么是NoSQL

可以理解为 not only SQL，是一种能同时accomodate a wide variety of data models 的database management 方法。

2. NoSQL支持SQL吗

是的，有些NoSQL database支持的语言有点类似SQL

### 现有的relational database注重两方面

1. data intergrity：数据完整性，通过schema加强
2. strict transaction semantics：防止并行程序形成 inconsistency

### NoSQL注重两方面

1. Elastic scaling：体积易于成长（应对web庞大的数据）
2. Simple operations：操作要方便

> 例子：shopping carts, user profiles, calendar data, customer data

### NoSQL种类

**key-value**： 用key更新value，例如 Redis, Oracle NoSQL, Amazon DynamoDB

**Document**： 用key更新document，例如 Couchbase，MongoDB

**Wide Column**： 用key更新 a collection of column families，例如 Apache Cassandra, HBase

### 背下来

![image-20220114190633311](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141906437.png)

![image-20220114191951988](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141919132.png)

![image-20220114192219213](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141922367.png)

Basically Available: 随时都available，对任何request

Soft state：系统状态会随时间改变（软的）

Eventual Consistency：停止接受input之后就变consistent

> 用ACD换了scalability

![](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141946982.png)

问题：NoSQL如何减少查询时间，时间复杂度O怎么变化，代价是什么？

1. 每个节点都存了其他结点range的信息，如果每个节点都知道其他的，那么时间复杂度就是O(1)。代价就是额外需要维护和更新range信息。
2. 每个Client可以存储不同节点的range信息。如果client知道所有的node，时间复杂度就是O(1)，代价就是要在各个Client上维护和更新range信息。

![image-20220114195534038](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141955166.png)

> 先找最近前驱，再找这个前驱的后继节点

## 圆环Chord

Chord is a distributed hashing proposal for internet scale applications.

Find the node for a key.

![image-20220114193817235](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141938387.png)

### Finger Table

![image-20220114193830265](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141938389.png)

The finger table contains m entries.

> 总结，找某个节点的第n个entry：
>
> 节点的数 + 2的（n-1）次方，向上找最接近的结点，最多只有第m个
>
> 如果相加超过2的m次方，就mod 2的m次方

# 内容梳理

> For semi-structured data:
>
> ​	XML databases
>
> For semi-structured data or documents:
>
> ​	NoSQL databases
>
> For graph data:
>
> ​	RDF databases

# XML

一共有这几种node

document

element

attribute

comment

text

> 和XML有关的几种语言：
>
> XPath: navigating through the tree，遍历节点树
>
> XQuery: XML的SQL语言
>
> XSLT：traversal and transformation 遍历和变换

## XPath

选择节点 selecting nodes

### 语法

1. 返回节点

**打开文档**： doc("books.xml")

**选择元素**：doc("books.xml")/bib/book  所有bib元素下的book元素

**选择元素（任意位置）**：doc("books.xml")//book

**选择所有元素**：doc("books.xml")/bib/*

**满足某个子节点值的结点**：doc("books.xml")/bib/book/author[last="Stevens"]

**返回每个book的第一个author**：doc("books.xml")/bib/book/author[1]

**返回所有author的第一个**：(doc("books.xml")/bib/book/author)[1]

2. 返回值

**判断，返回布尔值**：doc("books.xml")/bib/book/author/last="Stevens"

**返回book元素的year属性值**：doc("books.xml")/bib/book/@year

**返回所有 text node的值**：doc("books.xml")//text()

`c.f.`means 'confer', aka 'compare'

![image-20220114185550190](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141855313.png)

![image-20220114185603475](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141856597.png)

![image-20220114185616476](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141856609.png)

## XQuery

The FLOWER:

For: 选择一系列节点

Let: 绑定变量

Where: 条件过滤

Order: 排序

Return: 生成结果集

![image-20220114180407652](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141804774.png)

![image-20220114180451346](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141804458.png)

![image-20220114180524847](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141805979.png)

![image-20220114180708999](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141807117.png)

![image-20220114180731622](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141807759.png)

> 总结：用/访问节点，用//访问所有节点，用@返回属性值，用[attr=""]返回满足包含节点值的结点，用[1]返回第几个，用text()返回节点text内容
>
> 用$设置变量，用in遍历节点













