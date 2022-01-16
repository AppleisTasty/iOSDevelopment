# week4: Graph Data

RDF, SPARQL

SPARQL-through-SQL strategy 存储和评估

# 主要内容

1. RDF
2. SPARQL
3. algebraic view of SPARQL

# 考点

1. 用SPARQL写Query
2. 把SPARQL query转写成SPARQL algebra
3. Mappings
4. Relational Representation：用表表示RDF（N-ary property table，Triples Table, Property Tables)



## SPARQL转写Algebra

对应关系

AND 自然连接

OPT 左外连接

UNION 并集

FILTER 就是select

SELECT 就是project

ASK 就是逻辑非

### 如何从 SPARQL 变成 Algebra

![image-20220114211000722](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142110907.png)

最外面套上 $[\space] _D$ （这个表示的就是一个转化的function）

然后一层层往里面，最后套在三元组上



`cVars` certain variables

`pVars` possible variables

### 找pVars和cVars

![image-20220114212733624](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142127761.png)

因为`?y`不在union的那个集合里，所以合并后可能有记录没有`?y`

### 画operator Tree

![image-20220114213157555](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142131695.png)

## Modelling Triples with Tables

![image-20220114213803292](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142138422.png)

### n-ary property table

![image-20220114213840442](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142138582.png)

原理：推测entity是哪种类型，答案不唯一，没有的地方用NULL填补

好处：生成了一个relational的表

坏处：浪费太多空间，填补NULL，表太大

### Triple Table

存所有的三元组 SPO

![image-20220114214014430](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142140580.png)

这种方法下可以建立类似这样的索引表

![image-20220114214208162](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142142296.png)

好处：减少storage，坏处：增加查询时需要join的次数

> With a single table, queries will often require multiple self-joins, which are, in principle, often difficult to evaluate efficiency.

### property table

原理：按照每个property生成table

![image-20220114214457280](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142144428.png)

就是把property当做表明，里面放 subject 和 object

好处：不需要NULL，读取的时候可以只读到需要的部分

坏处：插入和删除可能会影响到schema的改变，缺少对未知predicate的支持（RDF是支持predicate可以是未知的）



# 内容梳理

## RDF

RDF as Graph Data Model

**Subject**: resource identifier

**Predicate**: property name

**Object**: property value (uri or literal)

![image-20220114202502490](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142025634.png)

### Query中可用的词

select 返回表

ask 模式匹配，返回布尔值

construct 返回一个匹配模板的graph

describe 返回描述

### Result中可用的词

distinct

reduced

order by

limit/offset

### SPARQL 语法

`BGP` Basic Graph Pattern 是一个set的TP（Triple Pattern） 

没有form的sql

用`@prefix`添加前缀

![image-20220114203042649](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142030736.png)

![image-20220114202949287](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142029423.png)

用句号点表示 AND，花括号表示group

![image-20220114203413187](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142034294.png)

用封号表示都用在同一个subject上

![](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142035522.png)

如何用filter：

![image-20220114203606842](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142036960.png)

> 用 SELECT DISTINCT 去掉重复值

用union来合并两个表

![image-20220114203650091](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142036212.png)

> if & binding & bound
>
> if(condition, then, else)
>
> bind(?x AS value)
>
> bound(?x) simply returns true if x is in the result, usually works with if.

用OPTIONAL表示 partial binding，类似左连接

![image-20220114203734806](/Users/tablee/Library/Application%20Support/typora-user-images/image-20220114203734806.png)

用ASK判断，返回布尔值

![image-20220114203752641](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142037774.png)

用construct返回RDF格式的结果

![](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142038052.png)

### 评估SPARQL query的步骤

1. 将 SPAQL query string 转换成 abstract syntax form
2. ASF 被map到 SPARQL abstract query（plan）（SPARQL algebra表示）
3. 这个query plan被用来评估

总结：转换成抽象表达式 - 生成plan - 评估



## Algebraic SPARQL

用问号开头表示变量`?x`

`BLU`： blank nodes, literals, URIs

### 核心语法

**SPARQL存在的几种操作**

- E FILTER R
- E AND E'
- E UNION E'
- E OPTIONAL E'

![image-20220114204918994](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142049119.png)

### Mapping

**compatible**:任意两个?x的关系在两个mapping中的结果是一样的

![image-20220114205652741](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142056875.png)

看两个Mapping是不是compatible：看相同的变量map出来的结果是不是一样的。

### Filter可用的

$\neg$

$\and$

$\or$

`bound(?X)`的意思就是判断有没有?x这个变量，返回布尔值

![image-20220114210030348](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142100498.png)

就是把变量带进关系里看对应的结果



![image-20220114221232993](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142212130.png)

自然连接：M和M'中找兼容的并起来

并集：直接把M和M' union起来

difference：找M中与M'任意一条都不兼容的

左连接：自然连接并上difference

select：把满足条件的规则留下

project：把包含指定变量的规则留下

## SPARQL 转换 SQL

![image-20220114215315358](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142153523.png)

原理：想象RDF是一张spo表

## 给Triple Table 生成index

有三种方式

1. Multiple Access Path (MAP) approach
2. HexTree approach
3. TripleT approach

目标就是index triple来支持 graph pattern 在 join 上的评估

### MAP方法

所有SPO的三三排列permutations都被index了

所有SPO的俩俩排列都被index了

![image-20220114215912466](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201142159599.png)

好处：MAP 和 HexTree的方法能帮SPARQL的where语句实现 targeted joins

坏处：非常冗余，占据永久空间。每个join都需要访问不止一个index，还要进行sort-merge join

### TripleT 方法

SPO分开设index，然后合并成一张表union

然后记住SPO分别对应的行的range

好处：involve less redundancy，存储空间也更小，减小了key的size，访问index次数也更少





















