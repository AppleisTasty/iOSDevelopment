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

The table is large, and query involves multiple times of self-join, which is hard for evaluation. May be many NULL values in the table and this consumes a lot of memory.

bad:



```
ASK{
	?AJ children ?a
	?MD children ?b
	where ?a > ?b

}
```

### 6. Three main dimensions involved in query optimization.

![image-20220116185042127](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201161850275.png)