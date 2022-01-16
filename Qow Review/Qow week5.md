# week5: Massive Parallelism

Map-Reduce

# 主要内容

1. Parallel Query Processing
2. Parallel Relational Databases
3. Schemes: Map-Reduce Engines
4. Qery Processing with Map-Reduce
5. Other current and Future Directions

# 考点





# 内容梳理

> Why parallel execution suits relational queries

![image-20220115185043678](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201151850822.png)

## Two forms of parallelism

**Pipelined (inter-operator)** & **Partitioned(intra-operator)**

> "inter" means "between", happening between two things
>
> "intra" means "within",  happening within a single thing

What do we have in parallelism:

1.  the operator (scan, sort, project )

> Relaitonal set operators: 
>
> SELECT
>
> UNION
>
> INTERSECT
>
> DIFFERENCE
>
> PRODUCT
>
> PROJECT
>
> JOIN
>
> That is why RA has the operator tree, that is why SQL needs to be interpreted into RA to do optimization.

### Pipelined

![image-20220115193056597](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201151930729.png)

<center>Pipelined model</center>

> This thread belongs to the operator "project"

Each operator has a own thread of control. So operators can execute at the same time.

May have `Blocking semantics`: waiting to read all the inputs so it can produce an output

In this graph, the `sort` operator is the "block". It needs to wait for all the scan results.



### Partitioned

![image-20220115194018691](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201151940851.png)

<center>Partitioned Model</center>

The resources are seperated, and the load is shared.

But the `sort` is still blocked. But since the load is shared, the negative impact of `blocking semantics`is alleviated.

> Commoditization（商品化）of Hardware tends to base on relatively-small processing units.
>
> Process data by partitioning it into smaller pieces.
>
> `commodity`商品

**DBMS offer the opportunity for both pipelined and partitioned parallelism.**

> Quick summary:
>
> Pipelined (inter-operator) : each operator has its own thread of control, so operators can execute at the same time.
>
> Partitioned (intra-operator): the resources are partitioned, each operator can have more than one thread, and the load is shared.
>
> `Sort` causes blocking semantics.

## Parallelism Goals and Metrics

### Goals

1. Linear Speed-up

Ideal speed-up tends to be stable, conforms to y = x, the ratio gets larger as size grows.

![image-20220115201115465](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152011575.png)

> speed-up is the ration between the time cost in the old smaller system and the new larger one.

2. Linear Scale-up

The data volume scales up but the elapsed time remains the same.

### Three Barriers to Linear Speed-Up and Scale-Up

1. start-up

Time cost to set up the parallel opeartors.

2. interference

Adding new parallel operators might slow down the existing one, because the job may be nearly done.

3. skew(倾斜)

adding parallel operators increases the size of each step, get shared too much, the time depends on the slowest parallel step.

## Parallel Architectures

Three possible designs:

1. shared-memory designs (servers access the same memory)

2. shared-disk designs (servers access the same disk)

3. **shared-nothing designs**  (This one is the most prevailing, servers)

![image-20220115202919581](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152029715.png)

Disk and memory unit are seperated. (Data is partitioned)

There are many processors(servers)

The data does not need to be transferred, only queries and the result do.

> As we can split up the data, though the data volume grows, we can arrange more processors and keep each partition with the same size. So we can ensure near-linear speed-ups and scale-ups.

Why it's the best:  

1. can deal with concurrent operation to update the data
2. minimize the burden on the interconnect, don't need to transfer the whole data set. Only queries and answer and some intermediate results are transferred.

## Parallel relational databases

### Data partitioning

评估parallel query的基础: 3个building blcok

![image-20220115210515528](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152105669.png)

It is to distribute the relation to multiple storage elements.

**Three basic partitioning strategies**

1. round-robin 循环

![image-20220115210923504](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152109622.png)

In a sequence, distributing tuples one by one to each disk. Start over after n times.

2. hash-based

![image-20220115211155651](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152111795.png)

According to each tuple's hash value, distribute them to the different disk.

3. range-based

![image-20220115211357645](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152113761.png)

Split the tuples with ranges, and each disk takes the corresponding range of data.

> 总结：
>
> three strategies for data partitioning
>
> 1. Round-robin: to give out one by one, in a sequence
> 2. Hash-value: the distribution based on the hash value of the tuple，tends to randomize data
> 3. Range based: each disk takes a set range of tuples, tends to cluster data
>
> associative access：模式匹配(A method of addressing a location by virtue of its data content rather than by its physical location)
>
> round-robin is poor for this, hashing is excellent for this,
>
> range-based is both good for sequential processing and associative access.

Problems:

range & hash partition may cause uneven to the data partitioning, including:

1. data skew: uneven distribution of data to the disk.
2. execution skew: uneven load across processing elements.

### Parallelizing Relational Algebraic Operators

**The goal** : no need for extra change, to use existing operator implementations

Only added three new mechanisms for "shared-nothing" architecture:

1. **operator replication**
2. **merge operator**
3. **split operator**

**Operator Replication**

There can be more than one operator processing at the same time.

In a range-based data partitioned parallel system. If query A only operates on range 1, then processing elements for range 2 & 3 can execute other queries simultaneously.

> replication 的规模受到 interference 的影响

**Merge and Split Operators** (also known as exchange operators)

This kind of operators exists at anywhere that has a need for merging and spliting data.

The merge operator merges the data from multiple inputs into one, and the split operator splits the data into n parts.

![image-20220115214905703](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152149864.png)

In this graph, there are two inputs for each join. One for relation A and the other for relation B. The red A takes odd values of tuples in relation A and the blue A takes the even values.

There are two disks, each disk has stored a partition of relation A & B.

There are data transferation after split operators (the intermediate results)

![image-20220115215606323](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152156469.png)

## Map-Reduce

The scheme for massively-parallel

Goal: To process massive load of data. Code with mapper and reducer to achieve parallel processing.

最广泛widespread应用的map-reduce方法是 Google's MapReduce/GFS.

### Map-reduce engine

map-reduce is highly-fault-tolerant, it is extremely resilient when nodes fail.

**programming**

```
map (in_key, in_value) -> (out-key, intermediate_value) list
```

```
reduce (out_key, intermediate_value list) -> out-value list
```

**mapper**

input is key-value pairs from file or database.

The function is to transform the latter each (in_value) into (out_key, intermediate_value) pairs.

<u>barrier</u>: out的key会hash，使得对应的reducer能接到。在分发给reducer之前，会被存到barrier，the barrier contains all (key-value) pairs.

There are many reducers, each with a "bucket number".

**The process**

The engine wait for map on all nodes to finish, then make the output of mappers available to the reducers.

Each reducer pull the relevant data from different mapper nodes.

mapper can be in parallel, so as reducer.

> Bottleneck: the reducers only work after all mappers have finished.

![image-20220115223011836](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152230994.png)

也就是说，reducer内部不需要对进来的数据做判断，因为进来的数据只有一类，直接做操作就行。

### Fault tolerance

The MapReduce engine has a "master program". It divvies up tasks. Detects the value pair that causes a mapper to crash, so to skip that when re-executing.

The mapper and reducer has re-execute mechanism if it failes at the first time.

### Optimization

1. If a mapper is doing really slow, the "master" will replicate its job to several additional components, and keep the results from the fastest one, ignore the rest.

2. The "combiner" is set on the same machine as the mapper (one machine may have more than one mapper), it can do a mini-reduce while waiting for other mappers to complete, so to save bandwith for the real reduce.

![image-20220115225813699](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152258858.png)

## Map-Reduce semantics for RA operations

1. selection
2. projection
3. union, intersection, difference
4. natural join
5. partitioned (or group-by) aggregation

> The mapper not only takes input from one relation, it can be a mixed input, part from relation A, and part from relation B.

### 1. Selection

Selection only needs mapper, the reducer simply emits what it takes.

![image-20220115232223534](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152322680.png)

Notice: the output is (t, t) pair, where t refers to the tuple.

So it is easy to obtain the tuple by taking only either the key or the value.

```
mapper
for each input tuple t
	if C(t) = true
		emit(t,t)
	
reducer
for each key t
	emit (t,t)
```



### 2. Projection

![image-20220115232848402](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152328537.png)

In the mapper, reduce the t to t', (keep only columns from the requirement)

In the reducer, eliminate the duplicates.

The reducer might take list of pairs, but only emit one pair.

```
mapper
	for each input tuple t
		emit (t', t') //t' is the columns preserved in the projection

reducer
 for each key t'
 	case more than one t' values:
 		emit (t',t')
 	case one t' value:
 		emit (t',t')
```



### 3. Union

![image-20220115233320256](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152333429.png)

The mapper emits what it takes, and the reducer emit only one pair (t, t).

(The reducer eliminates the duplicate)

```
mapper

for each tuple t
	emit(t,t)

reducer
for each key t
	case two t-values:
		emit(t, t)
	case one t-value:
		emit(t, t)
```



![image-20220116001623009](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201160016180.png)

### 4. Intersection

![image-20220115233550947](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152335093.png)

The mapper emits what it takes.

The reducer emits IFF it takes input as (t, [t, t]) (indicating that this tuple is in both relations), and emit one pair as (t, t)

```
mapper
for each tuple t
	emit(t,t)

reducer
for each key t
	if two t-values:
		emit(t,t)
```



### 5. Difference

![image-20220115234410635](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152344811.png)

The mapper now emits pairs with relation name as (t, 'R') or (t, 'S')

The mapper emits a (t, t) pair iff the input value list is ['R']. (not ['R', 'S'] or ['S'])

```
mapper

for each tuple t in R
	emit(t,'R')
for each tuple t in S
	emit(t, 'S')
	
reducer

for each key t
	if t-value list is ['R']
		emit(t,t)
```



### 6. Natural Join

![image-20220115235234909](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201152352071.png)

```
// in R(A,B) and S(B,C)
mapper

for tuple in R
	emit(b,('R',a))
for tuple in S
	emit(b,('S',c))
	
reducer

for each key b
	list = []
	for each ('R',a)
		for each ('S',c)
			list.append(a,b,c)
	emit(b, list)

```



### 7. Group by

![image-20220116002401383](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201160024533.png)

The mapper emits (a, b) pairs, a is the group by column, and b is the preserved column.

The reducers emits one pair (a, x) where x = function([b1,b2,b3,b4]), (e.g, function is sum, then x = number of b)

```
//assume project B, function on A
mapper

for each tuple t
	emit(a,b)
	
reducer

for each key a
	emit(a,x) //where x is the aggregation result on list of [b1,b2,...bn]

```



![image-20220116003013498](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201160030637.png)



















