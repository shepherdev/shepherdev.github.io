---
title: 了解JSON
date: 2020-04-20
toc: true
author: shepherd
categories: [前端,JavaScript]
tag: [JSON]
---

> [JSON](https://developer.mozilla.org/zh-CN/docs/Glossary/JSON) 是一种按照JavaScript对象语法的数据格式，这是 [Douglas Crockford](https://en.wikipedia.org/wiki/Douglas_Crockford) 推广的。虽然它是基于 JavaScript 语法，但它独立于JavaScript，这也是为什么许多程序环境能够读取（解读）和生成 JSON。 
>
> JSON可以作为一个对象或者字符串存在，前者用于解读 JSON 中的数据，后者用于通过网络传输 JSON 数据。 这不是一个大事件——JavaScript 提供一个全局的 可访问的 [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) 对象来对这两种数据进行转换。
>
> 一个 JSON 对象可以被储存在它自己的文件中，这基本上就是一个文本文件，扩展名为 `.json`， 还有 [MIME type](https://developer.mozilla.org/zh-CN/docs/Glossary/MIME_type) 用于 `application/json`.---MDN

<!-- more -->

## JSON对象

```json
{
  "squadName" : "Super hero squad",
  "homeTown" : "Metro City",
  "formed" : 2016,
  "secretBase" : "Super tower",
  "active" : true,
  "members" : [
    {
      "name" : "Molecule Man",
      "age" : 29,
      "secretIdentity" : "Dan Jukes",
      "powers" : [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name" : "Madame Uppercut",
      "age" : 39,
      "secretIdentity" : "Jane Wilson",
      "powers" : [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    },
    {
      "name" : "Eternal Flame",
      "age" : 1000000,
      "secretIdentity" : "Unknown",
      "powers" : [
        "Immortality",
        "Heat Immunity",
        "Inferno",
        "Teleportation",
        "Interdimensional travel"
      ]
    }
  ]
}
```

访问形式

```js
superHeroes.hometown
superHeroes["active"]
superHeroes["members"][1]["powers"][2]
```

## JSON数组

```json
[
  {
    "name" : "Molecule Man",
    "age" : 29,
    "secretIdentity" : "Dan Jukes",
    "powers" : [
      "Radiation resistance",
      "Turning tiny",
      "Radiation blast"
    ]
  },
  {
    "name" : "Madame Uppercut",
    "age" : 39,
    "secretIdentity" : "Jane Wilson",
    "powers" : [
      "Million tonne punch",
      "Damage resistance",
      "Superhuman reflexes"
    ]
  }
]
```

访问形式

```json
[0].powers[0]
[0]["powers"][0]
```

## 注意事项

- JSON 是一种纯数据格式，它只包含属性，没有方法。
- JSON 要求有两头的 { } 来使其合法。最安全的写法是有两边的括号，而不是一边。
- 甚至一个错位的逗号或分号就可以导致  JSON 文件出错。您应该小心的检查您想使用的数据(虽然计算机生成的 JSON 很少出错，只要生成程序正常工作)。您可以通过像 [JSONLint](http://jsonlint.com/) 的应用程序来检验 JSON。
- JSON 可以将任何标准合法的 JSON 数据格式化保存，不只是数组和对象。比如，一个单一的字符串或者数字可以是合法的 JSON 对象。虽然不是特别有用处……
- 不像 JavaScript 标识符可以用作属性，在 JSON 中，只有字符串才能用作属性。

## 序列化

`stringify()`: 接收一个JS对象作为参数，返回一个对应的JSON字符串。

```js
var person = {
  	name: 'shepherdev',
    age:19,
    height:1.60,
}
var myStirng = JSON.stringify(person);
/*
{"name": "shepherdev","age": 19,"height": 1.6}
*/
```

`stringify`可以传递三个参数，第一个是js对象，第二个是指定输出的对象的属性，可以使用数组，第三个是缩进，如

```js
JSON.stringify(person,['name','age'],'    ')
/*
"{
    "name": "shepherdev",
    "age": 19
}"
*/
```

第二个参数还可以传递一个函数，用来处理每个键值对

```js
JSON.stringify(person,
   	(k,v)=>{
    	if(typeof v === 'string') 
           	return v.toUpperCase();
    	return v;},
'    ');
/*
"{
    "name": "SHEPHERDEV",
    "age": 19,
    "height": 1.6
}"
*/
```

控制js对象的序列化数据，还有一个方法就是在对象里写一个toJSON()方法，里面返回应该处理的对象

```js
var person = {
    name: 'shepherdev',
    age:19,
    height:1.60,
    // 注意不能用箭头函数
    toJSON:function(){
        return {
            'Name': this.name,
            'Age': this.age
        };
    }
};
//"{"Name":"shepherdev","Age":19}"
```

## 反序列化

`parse()`: 以文本字符串形式接受JSON作为参数，并返回相应的JS对象。

```js
var myText = '{ "name" : "shepherdev", "age" : "19" }';
var myJSON = JSON.parse(myText);
```

它的第二个参数也可以接收一个函数用来处理解析的属性

```js
JSON.parse(myText,(k,v)=>{
	if(k === 'name'){
		return 'hello '+v;
	}
    return v;
})
// Object { name: "hello shepherdev", age: "19" }
```

