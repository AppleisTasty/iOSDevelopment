# XML

一些值可以放在属性里也可以单独设立元素存储

![image-20220119110931571](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191109653.png)

Attributes are in the element's tag (同一个element的attribute名字不能重复)

XML itself is not a markup language, bu we can specify markup languages/formats with XML

大小写敏感

### JSON 对比 XML

![image-20220119114425374](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191144449.png)

> 总结：XML 的 IR 是 tree-shaped 的，但是 JSON 不是完全tree-shaped的



![image-20220119115532705](/Users/tablee/Library/Application%20Support/typora-user-images/image-20220119115532705.png)

![image-20220119115545398](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191155475.png)

### DOM Tree: The internal representation for XML

DOM (Document Object Model) trees.

> DOM is an API for XML.

![image-20220119130300397](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191303509.png)
![image-20220119130312357](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191303412.png)

![image-20220119130327657](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191303733.png)

![image-20220119130403358](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191304467.png)

> SAX: Simple API for XML (DOM API)
>
> XML并非特别需要 DTD, well-formed XML 就不需要有DTD

## Is XML self-describing?

Yes, well-formed XML & JSON is basically self-describing.

"From the external representation we can derive the internal representation."

Well-formedness is the universal minimal schema.

> what is PSVI?
>
> Post Schema Validation Infoset. means the information resulting from an XML document having been schema validated.

## RelaxNG

simpler than XSD (XML schema)

compact syntax & XML syntax.

![image-20220119171551675](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191715748.png)

![image-20220119181233016](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191812106.png)

![image-20220119182023062](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191820147.png)

![image-20220119182155160](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191821232.png)

紧凑型和完整性语法对应：

注意符号可以放在括号外面，也可以放在括号里面（意思不一样）

![image-20220119182324635](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191823698.png)

另类写法：对应两条规则

![image-20220119184444676](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191844787.png)

> 如果用逗号隔开，顺序不重要；如果用&符号隔开，需要严格按照顺序

![image-20220119184602486](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201191846548.png)



















