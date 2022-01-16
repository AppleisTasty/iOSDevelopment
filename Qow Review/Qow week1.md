复习计划：

拎出主要考点

其次再看一些概念的东西（整理大概率会涉及的，尤其是分点的，优缺点以及性质的一些内容，一些图）

# Week1: Database

复习和加强DBMS知识

## 主要内容

1. DBMS简介：定义、语言、应用
2. DBMS分析：内部结构，优点缺点，趋势，结构上的变种
3. 关系模型
4. 关系查询语句（TRC: Tuple relational Calculus 元组关系演算, Relational Algebra关系代数，SQL: structured query language）语言之间的比较

# 考点

1. 写TRC
2. 写RAE
3. 写SQL

> Differences between declarative language and procedural language.
>
> In a procedural language, you define the whole process and provide the steps how to do it. You just provide orders and define how the process will be served. In a declarative language, you just set the command or order, and let it be on the system how to complete that order
>
> 简而言之：declarative只需要给命令，procedural需要给出过程的具体步骤
>
> with or without describing how to compute it
>
> DRC(domain relational calculus) & TRC(Tuple relational calculus) are declarative, RA(relational algebra) is procedural.
>
> SQL基于TRC，所以是declarative



> Algebra -> Procedural
>
> calculus -> declarative

关系查询的原理：

先给集合，根据条件筛选出满足条件的行，再投射列 （集合 - 条件 - 行 - 列）

`arity`度：就是有多少个属性（列）

`cardinality`势：就是有多少数量（行）



主键是primary key，除了主键外被设置成key的就是candidate key

`primary key` is `candidate key`

`candidate key` may not be `primary key`

> 如何判断是不是key：值可不可以重复

一个表的`foreign key`是另一个表的`primary key`，用来连接两张表

# TRC

特征：最外面用花括号括起来，里面有 $\exists$ $\in$

## 怎么写

**第一步：写目标结果集合**

$\{A|\space\space\space\space\}$

**第二步：找到需要用的集合并给他们命名，放在一起表示join**

$\{A|\exists S \in Students \space \exists E \in Enrolled\}$

**第三步：写入join的条件连接表，写入筛选的条件选出行，用括号（）包起来，全部用^连接**

$\{A|\exists S \in Students \space \exists E \in Enrolled (S.stdid = E.studentid \and E.grade \ne 'B') \}$

第四步：投射出需要的列

$\{A|\exists S \in Students \space \exists E \in Enrolled (S.stdid = E.studentid \and E.grade \ne 'B' \and A.name = S.name \and A.cid = E.cid) \}$

> 如何把所有的列都投出来，直接用A$\in$集合

$\{A|A \in City \ \and\ A.Population > 3000000 \and \ A.Population < 5000000 \}$

# RA

特征：用的 $\pi$ 和 $\sigma$ (sigma)

## 怎么写

**第一步：找到要用的表，把他们连起来**

$(Students \bowtie _{stdid = studentid} Enrolled)$

**第二步：用select筛选出需要的行**

$(\sigma _{grade \ne 'B'}(Students \bowtie _{stdid = studentid} Enrolled))$

**第三步：用project筛选出需要的列**

$\pi _{name,cid}(\sigma _{grade \ne 'B'}(Students \bowtie _{stdid = studentid} Enrolled))$



## 不同的符号

$\times$ 笛卡尔积(cartesian/cross product)

$\backslash$ 除法 difference：把第二个集合的内容在第一个集合里去掉

$\cap$ 交集 intersection

$\cup$ 并集 union

$\bowtie$ 连接 join 自然连接

$\rho$ 重命名 renaming

$\leftarrow$ 分配 assignment：把结果存到一个新的集合里

$\mathcal{G} _{count(*)}$ 聚合函数

![image-20220113210903656](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201132109787.png)

![image-20220113212009952](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201132120100.png)



第1步：将`instructor`重命名为`i`和`w`，即$\rho _i (instructor)$, $\rho _w (instructor)$
第2步：在关系`w`中找出`ID`12121的教师，即![\sigma_{w.ID=12121}(\rho_{w}(instructor))](https://math.jianshu.com/math?formula=\sigma_{w.ID%3D12121}(\rho_{w}(instructor)))
第3步：将关系`i`跟第2步中的输出关系做**笛卡尔积**

$\rho _i (instructor) \times \sigma _{w.ID=12121} (\rho _w (instructor))$

# RA in programming language

整体格式:`\命令_{参数}

```
\rename_{Country_Name}
\project_{Name}(
	\select_{Name = p_Name}(
		Country \join_{Code = p_Country} \rename_{p_Name,p_Country,p_Population,p_Area,p_Capital,p_CapProv} Province
	)
);
```



# 几种不同的连接

笛卡尔积：直接乘起来 catesian product/ cross product

自然连接：一种特殊的等值连接，所有名称相同的列的值都要相同 natural join

等值连接：只需要给出条件的列相等即可 equijoin

左外连接：自然连接+保留左表 left outer join

右外连接：自然连接+保留右表 right outer join

全外连接：自然连接+保留左右表 full outer join

（保留的行在另一表对应的行用null填充）



# SQL

SELECT S.name, E.cid

FROM  Student S, Enrolled E

WHERE S.stdid = E.studentid AND E.grade != 'B'

