### 1. What are the keys words used in a SPARQL query, and what are used for the result?

**Query中可用的词**

select 返回表

ask 返回是不是有结果的，返回布尔值

construct 返回一个匹配模板的RDF graph（三元组形式）

describe 返回描述

**Result中可用的词**

distinct

reduced

order by

limit/offset

### 2. 课外阅读 "Native XML databases", Key/Value stores (KVSs) Shortcoming of KVSs

1. Free text search is not supported
2. key/value tables must be created in advance, which requires knowing all possible queries in advance.
3. Joins are not supported and therefore must be programmed as part of application logic.
4. Joins require pulling and parsing entire documents, thereby slowing retrieval down.

### 3. 课外阅读 Well supported by a key-value store (DynamoDB)

1. Efficient access by key
2. Scalable to accommodate varying levels of demand.
3. Minimal tuning from database administrators. (只需要管理员做最小的优化)

### 4. 课外阅读 Some indicators that using a native XML database is the most suitable approach for an application.

1. Data is represented in XML predominantly and from the start.
2. The data is a merge from multiple, different resources.
3. Complex hierarchical relationships in data.
4. The schema is significantly evolving over time.

### 5. what are the benefits and drawbacks of n-ary property table, triple table and property table?

**n-ary property table**

good:

1. Generated a relational table

bad:

1. possibly too many NULLs, wasted too much space
2. The table is too large



**triple table**

good:

1. It's neat, and reduces the space consumed.

bad:

1. Single table,  too much self-joins in queries, difficult to evaluate efficiency.



**property table**

good:

1. no need for NULLs to fill in
2. less wasted I/O, can read only relevant attributes to the solution.

bad:

1. lack of support for unknown predicates (RDF supports unknown predicates)
2. Insert or delete may lead to schema change operations



```
ASK{
	?AJ children ?a
	?MD children ?b
	filter(?a > ?b)
}
```

### 6. Three main dimensions involved in query optimization.

![image-20220116185042127](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201161850275.png)

### 7. SQL 和 NoSQL

### 现有的relational database注重两方面

1. data intergrity：数据完整性，通过schema加强
2. strict transaction semantics：防止并行程序形成 inconsistency

### NoSQL注重两方面

1. Elastic scaling：体积易于成长（应对web庞大的数据）
2. Simple operations：操作要方便

> 例子：shopping carts, user profiles, calendar data, customer data



### 8. ACID 和 BASE

![image-20220114190633311](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162114416.png)

![image-20220114191951988](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162114515.png)

![image-20220114192219213](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162114084.png)

Basically Available: 随时都available，对任何request

Soft state：系统状态会随时间改变（软的）

Eventual Consistency：停止接受input之后就变consistent

### 9. How NoSQl database reduce lookup times(using chord)

![](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162115390.png)

### 10. Something about the "Chord"

**chord property**

![image-20220116212218661](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162122775.png)

**chord capabilities**

![image-20220116212241621](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162122762.png)

### 11. Steps for evaluating SPARQL query

### 评估SPARQL query的步骤

1. 将 SPAQL query string 转换成 abstract syntax form
2. ASF 被map到 SPARQL abstract query（plan）（SPARQL algebra表示）
3. 这个query plan被用来评估

总结：转换成抽象表达式 - 生成plan - 评估



### 12. 给Triple Table生成Index的三种方法

有三种方式

1. Multiple Access Path (MAP) approach
2. HexTree approach
3. TripleT approach

目标就是index triple来支持 graph pattern 在 join 上的评估

### MAP方法

所有SPO的三三排列permutations都被index了

### HexTree方法

所有SPO的俩俩排列都被index了

![image-20220114215912466](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162219924.png)

好处：MAP 和 HexTree的方法能帮SPARQL的where语句实现 targeted joins

坏处：非常冗余，占据永久空间。每个join都需要访问不止一个index，还要进行sort-merge join

### TripleT 方法

SPO分开设index，然后合并成一张表union

然后记住SPO分别对应的行的range

好处：involve less redundancy，存储空间也更小，减小了key的size，访问index次数也更少



### 13. Why shared-nothing parallelism is the best?

1. can deal with concurrent operation to update the data
2. minimize the burden on the interconnect, don't need to transfer the whole data set. Only queries and answer and some intermediate results are transferred.

> 简而言之：
>
> 1. 可以处理并行
> 2. 减少I/O压力，不用整个数据集传来传去



### 14. Three Building blocks for evaluating query in parallel

![image-20220115210515528](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201162226987.png)

1. relation的分配
2. operator间的pipeline
3. operator在processing element 间的copy

### 15. problems with Hash-based & range-based data partitioning

range & hash partition may cause uneven to the data partitioning, including:

1. data skew: uneven distribution of data to the disk.
2. execution skew: uneven load across processing elements.



### 16. Fault tolerance in map-reduce

### Fault tolerance

The MapReduce engine has a "master program". It divvies up tasks. Detects the value pair that causes a mapper to crash, so to skip that when re-executing.

The mapper and reducer has re-execute mechanism if they failed at the first time.



### 17. Optimization in map-reduce

### Optimization

1. If a mapper is doing really slow, the "master" will replicate its job to several additional components, and keep the results from the fastest one, ignore the rest.

2. The "combiner" is set on the same machine as the mapper (one machine may have more than one mapper), it can do a mini-reduce before sending it through the barrier, so to save bandwith for the real reduce.



### 18.

![image-20220116102445776](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201161024966.png)