# CSV

```
column name, column name, column name

value, value, value, value
```

### **CSVW**

```
{
	"@context": ...csvw
	"url": "file.csv"
	"tableSchema":{
		"columns":[
			{
			"titles":"country",
			"datatype":"string"
			},
			{
			
			}
		]
	}
}
```

# XML

XML的属性也不能重名

draconian error handling: 1 well-formedness error: BOOM

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
   <catalogue year="good">
      <area>Bicycles</area>
      <entries>
         <bicycles>
            <element>
               <make>Cube</make>
               <price>600</price>
               <size>XL</size>
               <type>mountain</type>
            </element>
            <element>
               <make>Cube</make>
               <price>750</price>
               <size>S</size>
               <type>road</type>
            </element>
            <element>
               <make>Raleigh</make>
               <price>650</price>
               <size>M</size>
               <type>mountain</type>
            </element>
         </bicycles>
         <parts>
            <element>
               <make>Brooks</make>
               <price>20</price>
               <type>saddle</type>
            </element>
         </parts>
</entries>
      <id>Bicycles234</id>
   </catalogue>
</root>
```

### XSD

```
<xs:schema ...>
	<xs:element name="catalogue">
		<xs:complexType>
			<xs:sequence>
				<xs:element name="area" type="xs:string"/>
				<xs:element name="entries" type="xs:string"/>
			</xs:sequence>
		</xs:complexType>
	</xs:element>
</xs:schema>
```

### Schematron

```
<pattern>
	<rule context = "catalogue">
		<assert test = "count(catalogue) > 1">
			There are more than one catalogue!
		</assert>
	</rule>
</pattern>
```



### RelaxNG

数据种类只有text

```
grammar{
	start = element catalogue {catalogue-content}
	catalogue-content = attribute age {text},
											element name {name-content},
											element address {text}+,
											element project{project-content}*
	project-content = attribute type {text},
										text
}
```

XML 的 IR 是 tree-shaped 的，但是 JSON 不是完全tree-shaped的

# JSON

JSON除了数字都要用双引号引起来，一个element就是这里的一个object

JSON单个对象内的键值对不能重名

```
{
	"catalogue": {
		"id": "Bicycles234", 
		"area": "Bicycles", 
		"entries": { 
			"bicycles": [
				{"make": "Cube", "type": "mountain", "size":"XL", "price": 600},
				{"make": "Cube", "type": "road", "size":"S", "price": 750}, 
				{"make": "Raleigh", "type": "mountain", "size":"M", "price": 650}
			],
			"parts": [
				{"make":"Brooks", "type":"saddle", "price":20}
			] 
		}
	}
}
```

### **JSON schema**

```
{
	"$schema":...
	"description":...
	"definitions":{
		"entries":{
			"description":...
			"type":"string"
			"properties":{
				"bicycles":{
					"type":"string"
				},
				"parts":{
					"type":"string"
				}
			},
			"required":["id","name","age"]
		}
	}
}

```

# RDF

### RDFS

can't be used to validate RDF, it's like adding some extra information!