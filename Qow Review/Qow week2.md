# Week2: Query Optimization and Evaluation

Query 优化的过程

# 主要内容

1. query处理的过程
2. 逻辑优化 logical optimization
3. query评估 evaluation
4. plan的选择
5. 最终decision的产生

# 考点

1. 运用equivalence law
2. 计算result的size
3. iterator（写代码）



## 运用equivalence law（rewriting阶段）

题目：给表和query语句

![image-20220114123754483](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141237664.png)

**第一步：**

画出 Algebraic Operator Tree，单纯根据节点放operator（注意表的大小）

![image-20220114123828268](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141238436.png)

**第二步：**

把 select 条件分解放进去

![image-20220114123956145](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141239312.png)

**第三步：**

把更小的两个表放在一起执行

![image-20220114124153452](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141241616.png)

**第四步：**

把等式的select的条件放进 cross product 里面变成 equi-join

![image-20220114124300912](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141243034.png)

**最后一步：**

添加project，每一步只保留需要的列

![image-20220114124335690](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141243834.png)

> 总结：
>
> 把表达式画成树，把select条件分发下去，把小的表放在一起product(列少或者行少)，把等式条件的select写进join里变成equi-join，在每个join后添加project只取需要的列。完毕



# 内容梳理

## Query processing

主要流程，三个部分component，五个模块module

![image-20220114112822743](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141128865.png)

1. query compiler：编译

parse: 解析query成tree

Translate: 转译成 algebraic expression 

2. query optimizer：优化

Rewrite: 转写成QEP (query execution plan)

select：选择用于QEP的algorithm & access method

3. query engine：执行

Interpretor Execute: 执行 procedural 的QEP

> AE就是我们的RA，把SQL转译成RA

### RA表达式和对应的operator tree

operator就是select, project, cross product这些执行操作的operator

![image-20220114114501387](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141145492.png)

> RA转换成树：把每个operator作为一个节点画二叉树即可。注意有自下往上的箭头。操作数是operand。



### 如何优化这个树operator tree

> 1. 把select分别先对其下的表执行，再把表乘起来。（先缩小表，再乘积）
>
> 把select的条件放进join里变成 $\theta$-join

![image-20220114115537377](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141155491.png)

从 logical 变成 physical

![image-20220114115737536](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141157694.png)

>project的写法：Project([columnA, columnB])
>
>Join的写法：NLoopJoin(eq(flightno, flightid)) 
>
>数据输入的写法：Scan(Flights)

总结：translate结果是一个RA表达式，RA表达式可以变成一个AOT(Algebraic Operator Tree)，并在optimizer部分被rewrite，最终变成logical QEP.

### 从 logical QEP 变成 physical QEP

是在optimizer的select阶段之后，每个operator都可以用不同的concrete algorithm(具体的算法)，选定算法后就变成了physical QEP

## Algebraic Equivalences 关系代数优化

将expression改写得更高效

`E'`念做 E prime

### 1. associative law 结合律

$(x \times y) \times z \leftrightarrow x \times (y \times z)$

### 2. commutative law 交换律

$(x \times y) \leftrightarrow (y \times x)$

利用以上两条可以有11种Rule

> 规则的宗旨：优先减少数据集的量

### R0

把select的条件放到join里面

![image-20220114121848393](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141218528.png)

### R1

多条件筛选，一个一个执行

![image-20220114121827272](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141218436.png)

因为每次筛选后数据集会越来越小，所以cost会变小

### R2

有多个筛选的时候，筛选力度更大的先做

![image-20220114121837321](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141218429.png)

$\theta _1$保留更少的数据集，减轻$\theta _2$的负担

### R3

project很多次的话，直接只保留最后一次project的列

![image-20220114121537857](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141215050.png)

### R4

先把列保留，再筛行

![image-20220114121706662](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141217852.png)

### R5

join 和 cross product 都是满足 commutative law 的

![image-20220114122539868](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141225050.png)

### R6（1+2）

如果条件只筛选一边，就先筛，再乘积。 commutative

![image-20220114122656608](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141226778.png)

![image-20220114122851097](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141228261.png)

### R7（1+2）

先把列投出来

![image-20220114122920608](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141229779.png)

因为单纯的笛卡尔积没有predicate,所以一般都用有条件的theta-join代替

### R8

交集 和并集都是可以交换的

![image-20220114123036401](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141230582.png)

### R9

一连串同样的操作都是满足结合律的

![image-20220114123126099](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141231251.png)

### R10

select的操作与交集、并集、除法 满足分配律

![image-20220114123206200](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141232205.png)

### R11

project操作与并集 满足分配律

![image-20220114123259451](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141232580.png)



> 总结：条件拆分，去除冗余，通过交换优先减少数据集，避免cross project，避免对全集进行处理



## Algorithmic Strategies

一个公共的算法策略:（这些算法遵循下列之一的策略）

1. Scanning：扫描数据，读入数据
2. Indexing：建立索引，方便查询
3. Partitioning：分开，把数据拆分成一系列更小的操作

## Query Evaluation Strategies

Operator和Operator之间的协议

1. Materialization：每个operator操作完的结果都暂时存在memory或disk，这个结果叫materialization

2. Pipelining：不存储中间结果的流传输。

## The Iterator Model 

用于pipelining的迭代器

在这个model这下，每个physical operator都使用下列function：

1. open：初始化数据
2. next：下一位
3. close：关闭并清理

## Physical Operators

类型（按从disk读取数据次数分类）：

1. One-pass algorithms：只从disk读一次
2. Two-pass algorithms：读两次（把结果写回disk）

> 课程一般默认是 one-pass algorithms

### Scan操作（使用iterator)

![image-20220114144235364](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141442533.png)

open读第一个block，以及第一个tuple

![image-20220114144453231](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141444359.png)

Next就是每次递增t，如果遇到t在一个block里读完了，那就移动b到下一个block，然后把t设置为该block的第一个tuple，否则就返回not-found。（这里close不做操作）

### Select操作（使用iterator）

![image-20220114144731900](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141447044.png)

使用这个iterator，iterator的读入是data collection

先open打开读入，再在next里面做判断，创建临时变量t，储存iterator的next。不为nil时进行判断t满足theta，满足就返回t；

循环完成就返回not-found

这里的close要把iterator关掉。

### Project 操作（使用iterator）

略

### Nested-Loop Join操作（使用iterator)

不是很高效的一种

![image-20220114145729674](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141457797.png)

**Hash-Join**

先建立一个哈希表（用于join的attributes的哈希表）

然后就是通过一个表建立哈希表，另一个在遍历的时候查这个表。

![image-20220114150251039](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141502180.png)

第一个insert两个参数，前一个是属性名，后一个是属性对应的值

**sort-join**

类似于哈希join，这个用个空间把内容都排序排好，然后两个表之间进行比对。

![image-20220115102659525](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201151026685.png)



### Set & Aggregate Operation （使用iterator)

set就是sort两张表，然后scan之后merge，规避重复的项

set 哈希的办法就是：两个table都建一张表，然后扫描一张table追加到另一个table，规避重复的项。

Aggregate指的就是counts，sums，smallest，largest 之类的

## Cost of Plans

计算plan的cost

`M`代表可用的内存块数

`B(R)`代表存R表用的内存块数

`T(R)`代表R表的tuple的数量

`V(R,A)`代表R表中A列 distinct value 的数量

## Size Estimation 大小估计

无法直接预测query plan的执行时间，其中之一就可以从结果集的大小来进行预测

> 不乘上表的大小就是selectivity，乘上了就是size

### Project 的 size

前提：需要知道投射的列的平均占用byte数量

![image-20220114151957916](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141519040.png)

投射语句占用的空间 = $\frac{表总大小(blocks) \times (投射的列平均大小之和(bytes))}{表的平均大小(bytes)} $

### Select 的 size（一般考这个）

![image-20220114152436881](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141524021.png)



1. 所有的都满足：T(R）x T(S)

![image-20220114153301098](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201141533288.png)

公式：$\frac{两表数量相乘}{用于连接的列最大的distinct的数量}$

## RA评估

### join orders

**Greedy algorithm**

让join中间过程的表的大小越小越好

小的表先join





































