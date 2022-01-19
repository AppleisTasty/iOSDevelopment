# XSD

XML Schema Definition

XML Schema

> each element and type can only be delared once

## Simple Type

xs:string

xs:decimal

xs:integer

xs:boolean

xs:data

xs:time

## Complex Type

xs:sequence (严格按照顺序)

xs:all (和sequence一样，但不要求顺序)

xs:choice （选择其中一个）

可以搭配 minOccurs 和 maxOccurs属性，默认是都出现一次

![image-20220119201350654](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201192013749.png)

mixed-content: 把 complexType的 mixed="true"，就可以混合

## PSVI

PSVI = DOM for XML document + XSD schema



## 对比 XSD 和 RelaxNG

![image-20220119201831865](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201192018946.png)

总结： 

XSD比RelaxNG更详细，RelaxNG不能定义数据类型，没有list



## Postel's law

be liberal in what you accept and conservative in what you send.































