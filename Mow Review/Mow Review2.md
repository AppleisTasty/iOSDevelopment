# week2: Semi-structured data

# Regular Expressions

1. *e1,e2* (concatenation)
2. *e1*|*e2* (choice)
3. e1* (repetition) : can be empty
4. e+ (1 or more)
5. e? (optional) : e|ε (empty)

**example**

> Which of the following strings matches this regular expression: (a,b)* , a?, b, a, b* (There may be multiple answers.)
>
> - [ ] abbbabababababab
> - [ ] abbbbb
> - [x] abababbabb
> - [x] abba
> - [ ] ababababbababbabb

# Trees

### Features

1. edges & nodes
2. edges are directed
3. finite (each tree has only finitely many nodes)
4. each node with at most 1 incoming edge (can be none: root node)

`ε` is used for the *empty* string, length = 0



Three traits of a tree:

1. each tree has only finitely many nodes
2. each tree has a root `ε`
3. the predecessor of a node is defined in a Tree (prefixed-closed)



**Example**

Draw the tree from the given function

![image-20220112201149737](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201122011851.png)

Just draw someting like this:

![image-20220112201208856](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201122012948.png)

### Three pain points of SQL queries

1. queries with many joins 
2. queries that are deeply nested
3. queries that are infinitely nested

(tricky to write, easy to get wrong)

(for infinite nests, impossible to write as nested joins, requires recursive joins)



# Semi-structured Data

**Why?**

to support

1. Web document view
2. DB strict structures

### Features

1. possibly nested set
2. field-value pairs

(not all fields may be required, carries its own description)

`{ }`: set

`email: "ittakesmetoolongtothink@gmail.com"`: field & value

![image-20220113123813026](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201131238164.png)

use a comma to seperate pairs.

> Does white space matter?

### Internal representation

✨✨✨✨✨

> How parsing and serializing does to the data.

**can be used to determine when two pieces of semi-structured data are the same, and what matters**.

The same external representaion can result in differnet internal representatiaons (depending on the schema used).

![image-20220118175046029](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201181750112.png)

最上面是internal，最下面是external **(这个表背出来)**

parsing: 解析, serializing 串行化

`internal`: **a presentation to the mind in the form of an idea or image**

`external`: **a formalism to encode various classes of designs as mathematical objects together with their most important properties**

> Question: What is internal repr and what is external repr?

`SSD`semi-structured data

![image-20220113185508367](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201131855510.png)

叶子节点：数据项

内部节点：无内容

边：域名



**object identifier** `&`

objects can refer to each other，加在set的前面 &object{}，不需要冒号。

![image-20220113191959048](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201131919150.png)



> Store semi-structured data:
>
> (various formalisms)
>
> XML
>
> JSON

They have different mechanisms for **selft-describing**. e.g. schema languages.

**schema** describes structure and datatypes.

## JSON

JavaScript Object Notation

To serialise/store/transmit JavaScript objects

> JSON objects can be serialised into **JSON** and **XML**.
>
> But in XML it needs more design choices. (e.g. element/attribute names?)

### 基本语法

Arrays: [1, 2, "one", "two"]

objects: {"one":1, "two":2}

> 方括号数组，花括号object

### JSON vs. XML

![image-20220118181711534](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201181817632.png)

XML 用的 elements

JSON 用的 objects （一般就是array和object的组合）

**Application using JSON** Ajax

## Self-describing & Schema Languages

"From the external representation one should be able to derive the corresponding internal representation."

"If one converts from an internal representation to the external representation and back again, the new internal representation should equal the old"

Give a base format and a specific document , to parse the data into an internal representation.

> JSON is a little bit more `self-describing` than CSV

### CSV

by providing the first line as the header with filed names

!!! Only JSON or XML itself cannot be `self-describing` enough, we need schemas!

## Schema

is a description

1. for SQL it describes: tables, their names and their attributes, keys, integrity constraints
2. for CSV it describes: columns, value range
3. for JSON it describes: structure (how objects are nested, which key is required), data (datatypes, restrictions)

### A schema describes

1. what is legal
2. what is expected
3. what is assumed (default value)

Two modes fro using a schema

1. descriptive (describing documents for other people)
2. prescriptive (规定的，指定的)

## Schema languages

### 1. CSV (CSVW)

![image-20220118203915444](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201182039541.png)

> 特征：
>
> 第一行引用csvw (Link header)
>
> 第二行链接文件
>
> 第三行 tableSchema
>
> (Note that you can use regular expressions to describe column titles or values in CSVW)

It supports:

1. built-in datatypes
2. XSD (XML Schema Data Types)

### 2. JSON Schema

![image-20220118211205195](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201182112283.png)

```
{
	"$Schema": ,
	"title": ,
	"type":"object",
	"properties":{
		"id":{
			"type":integer
		}
	}
	"required":["id","name","price"]
}
```



![image-20220118211241206](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201182112309.png)

![image-20220118212318686](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201182123781.png)

![image-20220118212443443](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201182124520.png)

































