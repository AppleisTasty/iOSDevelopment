# Review Steps

1. 通篇浏览课程内容（2天）
2. 研究题型（quiz，往年试卷，essay）
3. 熟记

是什么 -> 有什么区别 -> 做法

# Week1: 

# Key Concepts

| Data Models               | Flat Files              | Table Based (The relational model) | Tree Based                                              | Tree based                           | Graph Based |
| ------------------------- | ----------------------- | ---------------------------------- | ------------------------------------------------------- | ------------------------------------ | ----------- |
| IntRepr                   | list of strings, arrays | table                              | Tree; list of strings; objects and arrays with nesting. | DOM tree; Infoset, XPath.            |             |
| ExtRepr                   | CSV file; text          |                                    | JSON file/snippet; text                                 | XML                                  |             |
| Schema(langs)             | CSVW                    | SQL                                | JSON schema                                             | DTD; XML Schema; Schematron; RelaxNG |             |
| Manipulation Tool         |                         |                                    |                                                         | XQueries; XSLT; XPath                |             |
| Data formats & Robustness |                         |                                    |                                                         |                                      |             |

> JSON is not "made for" trees, its IR can be in other forms.

internal repr: XML  to dom tree, PSVI

DOM = Document Object Model

The tool used to parse&validate text against schema is called `validators` or `schema-aware parsers`

![image-20220118112258645](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201181123187.png)

JSON internal Repr : Trees, a list of strings

![image-20220119214735787](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201192147857.png)

# Idendity

| File           | identify                         |
| -------------- | -------------------------------- |
| CSV            | line number                      |
| Excel          | row label                        |
| Relation Model | distinguishing attributes (keys) |



# formalism & language

| data model       | formalism          | language                        |
| ---------------- | ------------------ | ------------------------------- |
| relational model | relational algebra | SQL (Structured query language) |

JSON and XML are two formalism for semi-structured data.

### Data Life-span:

1. Transient 临时的，中间态，未存入数据库
2. **Persistent**



### Data Structure

**Opaque 不透明的**

videos, text, audio, images (unstructured)

**Transparent 透明的**

numbers, strings (semi-structured) 可直接用于编程

> Our focus is on `persistent`,`(semi-structured)`data



### What is semi-structured data?

Examples: XML and other markup languages

Definition: A third type of data, between structured and unstructured data. It does not have a rigid schema, so it does not conform to a data table or relational database structure. But it has internal semantic tags or metadata or markings, that enable analysis.



### What is structured data?

Examples: Excel files or SQL databases.

Definition: **data that adheres to a pre-defined data model** and is therefore straightforward to analyse.



### What is ACIDity？

**Atomicity**: 原子性, either performed or not, all changes as on operation.

Consistency: 一致性, Data changes should be consistent on both ends. 例如银行账户转钱，两边钱的总和要一致。

Isolation: 独立性, transaction 之间相互不影响

Durability: 持久性，transaction 成功后，变化确定且不可逆。



### Data on the Web 例子

**on the web**

data.gov.uk

IMDB.com

uniprot.org



**behind the web**

amazon.com

wordpress.com



Differences:  The purpose of the website is different, whether just to share the data set, or using the data set to do other stuff, e.g. selling products.



### Data model consists of...

1. an underlying data structure: Core data model
2. data integrity
3. data manipulation
4. (data sharing)



### What's difference betwen Data Model, Core data model and domain model/data format?

There are four core data model: flat files, table based, tree based, and graph based.

Data model can build on any of the core data model, it's a variant.

domain model/data format is a domain-specifc data model that build upon the core data model.



### Conceptual Model?

Example: ER diagram

it is independent of (core) data model.

it is often used for scoping 审查 & clarification



ER diagram

方形rectangle实体，圆形circle属性，菱形diamond关系

![IMG_4F85E4B732D6-1](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201110614592.jpeg)



### What is Polyglot Persistance?

多语言持久性

储存数据的时候最好对不同的数据使用不同的存储手段， picking the right tool for the right use case.

![image-20220111060819554](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201110608772.png)

> One core data model can have different acutal formats.



# CSV

> comma 逗号，colon 冒号，exclamation mark 叹号
>
> quesiton mark 问号，period 句号

CSV is comma-separated (aka. comma delimited)

### Definition

CSV stands for Comma Separated Values, is a plain text file that contains a list of data.

### Features

First line is the header line. -> column names

one record per line

values separated by a comma

we may need to quote cells

### Pain points

1. Sorting & Filtering is not handy. Operations are based on columns. Can't sort by part of the value. ( need python to do that)
2. Tricky to deal with multiple values at one column in one record. ( two ways to solve this: add more columns or create more flat files)
3. Not so convenient to link two or three spreadsheets. ( sorting in one file can destroy the link)



# The Relational Model (Table-based)

it is a core data model, **table is the core data structure**

`Relation` = `Table`

a table is a set of tuples. `n-typle` = n-ary list of values

Table name `R`, Attributes(column)`An`, Tuple(Row)

列是arity/degree, 行是cardinality

Relations are FOL predicates. Meaning the relation is the prediate R, with attributes R(A1,A2,A3) has a value True/False, if it's true meaning (A1,A2,A3) belongs to this relation.

### 范式

1NF (1st normal form): 每一列的每个数据项都是原子的，不能是数组、集合，若有多个需要拆分成多个属性。

2NF (2nd normal form): 在第一范式的基础上，完全依赖于主属性。（码就是主属性的集合）

> 例如表属性有(学号，课程号，学生院系，课程成绩)。其中学号和课程号是主属性（用于区分记录），如果满足第二范式，那么定位每一个非主属性都需要同时利用这两个主属性。这里学生院系就不满足这个条件，以为学生院系可以单单通过学号直接定位。

3NF 第三范式：在2NF的基础上，没有传递依赖。

> 例如，前面提到的关系模式：基本信息（学号、系部、学生住处）有下列函数依赖：
>
> 学号->系部
>
> 系部->学生住处
>
> 学号->学生住处
>
> 我们可以看到基本信息表中存在非主属性对码的传递函数依赖。所以，该基本信息表最高只符合2NF的要求，它不符合3NF的要求。
>
> 为了消除该传递函数依赖，把基本信息表分解为两个关系模式：
>
> 所在系部（学号，系部）
>
> 住宿（系部，学生住处）
>
> 所在系部表的码为学号，住宿表的码为系部。
>
> 在分解后的关系模式中，既没有非主属性对码的部分函数依赖，也买有非主属性对码的传递函数依赖，在一定程度上解决了上述四个问题。



### A relation formalism(规范) with relational language

Formalism contains: syntax & semantics (语法和语义)

Schema languages & Manipulation languages & Query languages.

**Schema**

>  set constraints, create empty tables

CREATE TABLE table_name

**Manipulate/Update**

> generate new tables

INSERT INTO table_name

DELETE FROM table_name

UPDATE table_name

**Query**

SELECT ... FROM table_name

> SQL operations are largely(主要的) closed over tables (operated within a table)
>
> SQL has no mechanisms for conceptual level (e.g. ER diagrams)

%任意匹配(any match) _(single match)



### 几种join

**Natural Join 自然连接**

是一种特殊的等值连接，将名字相同的属性进行连接，在结果中消除重复的属性列。

```sql
select * from table1 natural join table2
```

**Inner Join 内连接** (inner可省略)

就是cross join加个条件 (用on添加条件)

```sql
select * from table1 inner join table2 on table1.A=table2.E
```

**Outer Join 外连接（left,right,full)** (outer 可省略, mysql不支持)

就是把两个表进行自然连接，然后left就是保留左表全部内容，right就是保留右表，以此类推。因为原本要舍弃但是被保留的行，在另一表对应的列都用null填充（用on添加条件）

![image-20220111095220738](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201110952875.png)

```
select * from table1 full join table2 on table1.C = table2.C
```







# Quiz1

### What is unsigned in database

unsigned 为非负数，通常可以增加数据的长度。

### bag和set的区别

bag是无序集合，可有重复元素，不能保留顺序

set默认是无序集合，不能有重复元素，添加相同元素会覆盖旧的元素。可以要求被排序。

### SQL NULL value

SQl 的空值可以代表哪些内容

1. value not (yet) known 未知值
2. an attribute that is not applicable to the current row 当前不适用的值
3. the empty set 空值

### char(x) 和 varchar(x) 的区别

> varchar 是变长度，实际存多长就占多少空间
>
> char是固定长度，设定多少就占多少
>
> 没有哪个有绝对优势，需要具体衡量



### Total participation & Partial Participation

https://www.tutorialspoint.com/dbms/er_diagram_representation.htm





