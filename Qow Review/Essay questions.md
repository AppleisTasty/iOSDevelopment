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

1. wasted too much space
2. possibly too many NULLs
3. The table is too large



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

