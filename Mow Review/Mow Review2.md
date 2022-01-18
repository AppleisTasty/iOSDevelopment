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

**can be used to determine when two pieces of semi-structured data are the same, and what matters**

![image-20220113185042879](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201131850986.png)

最上面是internal，最下面是external

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









