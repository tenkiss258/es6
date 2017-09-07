# es6
保存我自己的es6练习

## 第一章 let和const命令
### let
1. let声明的变量只在其所在代码块有效
2. for循环可以使用let,每一次循环的let 值都是重新声明的变量。
3. let不存在变量提升，所以let声明的变量只能先声明，在使用，不能先使用再声明。
4. 在代码块内，使用let命令之前，该变量在代码块中是不可用的。
5. let不可以在代码块中重复声明。

### 块级作用域
1. 内层变量会覆盖外层变量
2. 用来计算的循环变量会变为全局变量
3. 因为快级作用域的出现，立即执行匿名函数就没必要了。

### const
1. 声明一个常量，一旦声明，常量的值不可以改变。
* 保存的只是地址，比如数组和对象，它里边还是可以插入值的
2. 只能在声明之后使用。
3. 不可重复声明。
4. const声明的变量只在其所在的代码块有效。

#### es6有六种声明变量的方式
```
var function let const import class
````

### 顶层对象的属性
1. 在浏览器指window对象，在Node指global对象。
2. 在全局环境使用let声明的变量不会成为顶层对象。

#### 以下两个方法为顶层对象兼容。
```
// 方法一
(typeof window !== 'undefined'
	? window
	: (typeof process === 'object' &&
	  typeof require === 'function' &&
	  typeof global === 'object')
	? global
	: this);

// 方法二
var getGlobal = function () {
	if (typeof self !== 'undefined') { return self; }
	if (typeof window !== 'undefined') { return window; }
	if (typeof global !== 'undefined') { return global; }
	throw new Error('unable to locate global object');
};
```

#### 跨模块常量
* 必须在es6环境下才可以使用，否则会报错。
```
	//constants.js模块
	export const A=1;
	export const B=2;
	//test1.js模块 
	import *as constants from './constants';
	console.log(constants.A);
	//test2.js模块
	import {A,B} from './constants';
	console.log(A);
```


## 第二章 变量的解构赋值
### 数组的解构赋值
* 数组的解构是按顺序的
```
// 对号入座，一一对应。
var [a,b,c]=[1,2,3];
// 加省略号代表本数组，只能给最后一位加省略号,赋值多于出来的都会放在数组里。
// 那个省略号为...运算符。详情参考Iterator接口
var [a,b,...c]=[1,2,3];
// 空值也占位置
let [x, , y] = [1, 2, 3];
// a=1,b=2,d=4;
let [a, [b], d] = [1, [2, 3], 4];
// 可以添加默认值 默认值必须已经声明。如果它的值是null将不执行默认值
let [a,b="b"]=["a"];
```
### 对象解构
* 变量必须与属性同名，对象的结构不需要按顺序
```
// foo为匹配的模式，aa才是变量，foo只是模式
let { foo:aa,bar:bb } = { foo: "aaa", bar: "bbb" };
// 还可以嵌套
var node = {
  loc: {
    start: {
      line: 1,
      column: 5
    }
  }
};
var { loc, loc: { start }, loc: { start: { line }} } = node;
line // 1
loc  // Object {start: Object}
start // Object {line: 1, column: 5}
// 也可以指定默认值 如果它的值是null将不执行默认值
var {x: y = 3} = {x: 5};
y // 5
var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
// 如果解构失败，变量的值等于undefined。
let {foo} = {bar: 'baz'};
foo // undefined
// {不能写在行首，不然javascript会理解为代码块
let x;
({x} = {x: 1});
//数组本质是特殊的对象，因此可以对数组进行对象属性的解构
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```
### 字符串解构
* 字符串解构是因为此时字符串被转换为一个类似数组的对象
```
// 类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
let {length : len} = 'hello';
len // 5
```
### 数值和布尔值的解构赋值
* 解构赋值时，如果右边是数值和布尔值时，则先回转换为对象
// 这个看不懂。
### 函数参数的解构赋值
```
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3
```
### 解构赋值的用途
>1. 交换变量的值
>2. 从函数返回多个值
>3. 函数参数的定义
>4. 提取JSON数据
>5. 函数参数的默认值
>6. 遍历map结构
>7. 输入模块的指定方法

## 第三章 字符串的扩展
1. 字符串的Unicode表达式
* Unicode码一般为\uXXXX,但有的时候回超出，那样就无法识别，遇到这样的可以使用{}来解决
2. codePointAt()
* 它与charCodeAt()类似，它可以返回32位的UTF-16的正确编码。返回的是十进制的，如果需要别的进制的使用toString(进制数)来改变
3. fromCodePoint()
* 这个方法定义在String对象上，参数为Unicode编码，返回字符串。
4. 字符串的遍历器接口
* 字符串的Unicode编码可以使用for...of循环遍历。普通的for循环无法遍历大于UTF-16 32字节的Unicode编码。
5. at()
* 用于获取字符串的字符。返回正确的字符，它可以识别Unicode编码大于0xFFFF的字符。
6. normalize()
* 可以使Unicode编码大于0xFFFF的字符的长度合并成一个汉字的长度。
7. includes() startsWith() endsWith()
* 是否找到了参数字符串/参数字符串是否在原字符串的头部/参数字符串是否在原字符串的结尾。（它们返回的是布尔值，可以添加第二个参数，表示开始搜索的位置）
8. repeat()
>1. 返回一个新字符串，参数表示重复n次。
>2. 参数如果是小数，向下取整。
>3. 参数不可以是负数，也不可以是无穷数。-0,NaN都会视为0
>4. 如果是字符串，则会先转换成数字。
9. padStart() padEnd()
* 用于头部补全/用于尾部补全
* 可以添加两个参数，第一个参数为最小的位数。第二个参数为补全的字符串。
* 主要用途：补全数值长度。另一个用途是提示字符串格式。
10. 模板字符串
* `内容` 使用`来定义字符串，定义的字符串可以换行
* 模板字符串中嵌入变量，需要将变量名写在${}里边。{}里边可以放任意的javascript变量，函数，对象。

## 第四章 正则的扩展
### RegExp构造函数
1. 它里边的参数有两种情况
```
// 第一种
new RegExp('xyz', 'i');
// 第二种
new RegExp(/xyz/i);
// 错误的
new RegExp(/xyz/, 'i');
// 正确的
new RegExp(/abc/ig, 'i');
```
2. 











## 第十五章 Generator函数的用法
### 基本用法
1. 在函数function后边加*号。
2. 内部有yield命令。执行到这个命令的时候，它会直接返回，下次调用会继续执行下一个yield命令。
3. 必须使用next()方法调用。return语句在yield命令之后仁执行，return之后的yield不再执行。里边可以传参，参数代表它上一个yield返回的值。next()第一个传参无效。它会返回value和done，value是返回的值，done返回的是布尔值，false表示遍历还没有结束。
4. 如果yield当表达式来用的话，需要加括号。



















* 以上内容借鉴于es6标准入门第二版和es6.ruanyifeng.com /2.let和const命令，最下边还有组件和包的顶层对象兼容方法。
