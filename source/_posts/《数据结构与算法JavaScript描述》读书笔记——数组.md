---
title: 《数据结构与算法JavaScript描述》读书笔记——数组
date: 2017-08-13 20:41:58
tags: 
  - 笔记
  - 《数据结构与算法JavaScript描述》
---

> 关于数组的基本使用，当作手册用了

<!-- more -->

> - 数组是特殊的对象，数字索引在内部被转为字符串，相比其他语言效率更低。
> - 数组是引用类型，将一个数组赋值给另一个数组时，仅仅是拷贝了该数组的指针，即*浅复制*。
> - 下文arr代表一个数组。参数中*号代表必须。-->代表结果。

---

### 1. 创建方式
```js
var arr1 = [1,2,3];//效率高
var arr2 = new Array(1,2,3);
//数组长度
arr.length;
//判断是否数组的方法, ECMA5,IE9+
arr.isArray() === true;
//兼容方法
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```
### 2. 操作方法
```js
//测试变量
var a,b,c,d,e;

1. 查询
arr.indexOf();//正序查询索引，返回第一个值所在位置，未找到则返回-1
arr.lastIndexOf();//倒序查询
//e.g.
a = [1,2,3];
a.indexOf(2);//-->1
a.indexOf('2');//-->-1 说明执行的是全等匹配 '==='

2. 转字符串, 不影响数组本身
arr.toString();
arr.join();
//e.g.
a = [1,2,3];
b = a.toString();//b-->'1,2,3'
c = a + '';//c-->'1,2,3' toString()的隐式转换
d = a.join();//d-->'1,2,3' 默认值','
e = a.join('');//e-->'123'

3. 并集、子集 返回新数组，不影响数组本身
arr.concat(arr1,arr2,...);//arr1...为要合并的数组
arr.slice(start,end);
//e.g.
a = [1,2,3];
b = [4,5];
c = a.concat(b);//a-->[1,2,3] c-->[1,2,3,4,5]
d = a.slice(1,2);//a-->[1,2,3] d-->[2,3]

4. 元素增删
arr.push(el1,el2,...);//末尾添加一个元素 返回新长度
//e.g.
a = [1,2,3];
b = a.push(5);//b-->4 a-->[1,2,3,5]

arr.unshift();//开头添加一个元素 返回新长度
//e.g.
a = [1,2,3];
b = a.unshift(5);//b-->4 a-->[4,1,2,3]

arr.pop();//末尾移除一个元素 返回被移除的元素
//e.g.
a = [1,2,3];
b = a.pop();//b-->3 a-->[1,2]
c = [].pop();//c-->undefined a-->[] 空数组的情况

arr.shift();//开头移除一个元素 返回被移除的元素
//e.g.
a = [1,2,3];
b = a.shift();//b-->1 a-->[2,3]
c = a.shift();//c-->undefined a-->[] 空数组的情况

5.特殊增删
arr.splice(index,num,el1,...);//index为索引，num为从索引开始要剔除的元素个数,el为剔除之后要添加的元素
//e.g.
a = [1,2,3];
b = a.splice(1,1,10);//a-->[1,10,3] b-->[2]
//返回剔除的那部分数组，同时修改了数组本身

6.排序
arr.sort();//按排序规则将每个元素转换成字符后进行排序，
arr.reverse();//仅仅是将数组的顺序颠倒，和sort()有本质区别

//e.g. 字符串间根据字符集码位排序，可用(data + '').charCodeAt()查询字符集码位
a = ['a','b','?'];//-->[]
a.sort();//a-->['?','a','b'] '?'-->63, 'a'-->97, 'b'-->98
//e.g. 当第一个字符相等时，比较第二个，直到比较出结果
b = ['a1', 'a0', 'a00'];
b.sort();//b-->['a0', 'a00', 'a1'], '0'-->48,'1'-->49

//e.g. 不同基础数据类型间的排序,很尴尬
c = [null,'a',1,undefined,true,false,{},[]];
c.sort();//c-->[[], 1, {}, 'a', false, null, true, undefined]
//[]-->''-->空字符最小, '1'-->49, {}-->'[object Object]'-->'['-->91, 'a'-->97
//'false'-->'f'-->102, 'null'-->'n'-->110, 'true'-->'t'-->116, 'undefined'-->'u'-->117 "
//中文拼音排序函数，依赖操作系统和浏览器内核，sql之所以能方便oder by就是因为明确定义了排序字符集
function compare1(char1, char2) {
    return char1.localeCompare(char2);
}

//e.g. 数字之间排序，同样是将数值转换成字符串，然后排序，看个奇葩的
d = [1,-1,0,Number.MAX_VALUE,Number.MIN_VALUE,Number.POSITIVE_INFINITY,Number.NEGATIVE_INFINITY,NaN];
d.sort();//c-->[-1, -Infinity, 0, 1, 1.7976931348623157e+308, 5e-324, Infinity,NaN]
//'-1'和'-Infinity'-->'-'-->45,'0'-->48,'1'和'1.7976...'-->'1'-->49, '5e...'-->'5'-->53
//'Infinity'-->'I'-->73,'NaN'-->'N'-->78
//数值排序的函数
function compare2(num1, num2) {
    return num1 - num2;//倒序则num2 - num1
}

```
### 3. 迭代方法

1. [arr.forEach(cal[, thisArg])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) (**ECMA5,IE9+**)

```js
//该方法遍历数组每一项，循环结束返回undefined。
//e.g.
a = [1,2,3];
a.forEach(function(o, i, arr) {
    console.log(o, i);
}, this);
//o为当前执行项，i为索引，arr为当前数组，可选thisArg参数用于改变执行函数的this上下文。
```

2. [arr.every(cal[, thisArg])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every) (**ECMA5,IE9+**)

```js
//该方法遍历数组每一项，遇到false立即返回false，停止循环，类似逻辑&&，每一项都为true则循环结束返回true。
//e.g.
a = [2, 4, 5];
a.every(function(o, i, arr) {
    return o % 2 === 0;
});//-->false
b = [2, 4, 6];
a.every(function(o, i, arr) {
    return o % 2 === 0;
});//-->true
```

3. [arr.some(cal[, thisArg])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some) (**ECMA5,IE9+**)

```js
//"该方法遍历数组每一项，遇到true立即返回true，停止循环，类似逻辑||，每一项都为false则循环结束返回false。
//e.g. 
a = [2, 3, 5];
a.every(function(o, i, arr) {
    return o % 2 === 0;
});//-->true
b = [1, 3, 5];
a.every(function(o, i, arr) {
    return o % 2 === 0;
});//-->true
```

4. [arr.reduce(cal[, initialValue])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) (**ECMA5,IE9+**)

```js
//该方法轮询数组中的每一项，将回调函数上一次return的操作结果作为下一次操作的第一个参数。
//可选参数initialValue作为轮询的初始值
//e.g.
a = [[1,2],[3,4]];
b = a.reduce(function(previous, current) {
    return previous.concat(current)
}, [0]);
//b-->[0,1,2,3,4] 实现数组扁平化
```

5. [arr.map(call[, thisArg])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) (**ECMA5,IE9+**)

```js
//该方法遍历数组每一项，执行回调函数，然后返回一个新的数组。
//e.g.
a = [2,3,4];
b = a.map(function(o, i, arr) {
    return o*o;
})//b-->[4,9,16]
```

6. [arr.filter(call[, thisArg])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) (**ECMA5,IE9+**)

```js
//该方法遍历数组每一项，验证表达式，实现过滤操作，返回表达式结果为true的项组成的新数组。
//e.g.
a = [2,4,5];
b = a.filter(function(o, i, arr) {
    return o % 2 === 0;
});//b-->[2,4] 
```

### 4. Tip

```js
1. 清空数组的三种方法
//e.g.
a = [1,2,3];
a.splice(0, a.length);
//这种方式不推荐

b = [1,2,3];
b.test = 't';
copyB = b;
b.length = 0;
//b-->[], copyB-->[] 对象引用同时被清空 
//b.test-->'t', copyB.test-->'t' 但属性都被保留  

c = [1,2,3];
copyC = c;
c = [];
//c-->[] 数组被清空，该方法性能最好
//copyC-->[1,2,3] 对象引用并未受到影响

```