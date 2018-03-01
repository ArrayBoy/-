
# JavaScript编码规范
[页面规范](#页面规范)
[基本规范](#基本规范)
* [变量](#变量)
* [常量](#常量)
* [分号](#分号)
* [块内函数声明](#块内函数声明)
* [封装基本类型](#封装基本类型)
* [with](#with)
* [for-in循环](#for-in循环)
* [多行字符串](#多行字符串)
* [Array和Object直接量](#Array和Object直接量)
* [修改内置对象的原型](#修改内置对象的原型)

[JavaScript编码风格](#javascript编码风格)
* [命名](#命名)
  * [通用命名](#通用命名)

[参考文档](#参考文档)
## 页面规范

 1. 所有的JS文件都要放在页面底部加载（除了特殊JS文件）
 2.  推荐一个页面或者模块对应一个JS文件
 3. 所有全局的函数可以封装在一个JS文件

## 基本规范

#### 变量
声明变量必须加上 var 关键字，避免泄漏到 Window 中。
#### 常量
常量的形式如: `NAMES_LIKE_THIS`, 即使用大写字符, 并用下划线分隔。

对于基本类型的常量, 只需转换命名。
```javascript
/**
 * The number of seconds in a minute.
 * @type {number}
 */
var SECONDS_IN_A_MINUTE = 60;
```
对于非基本类型, 使用 @const 标记。
```javascript
/**
 * The number of seconds in each of the given units.
 * @type {Object.<number>}
 * @const
 */
var SECONDS_TABLE = {
  minute: 60,
  hour: 60 * 60,
  day: 60 * 60 * 24
}
```
#### 分号
任何情况下不要漏掉分号，遗漏分号有时会出现错误的结果, 所以确保语句以分号结束。
#### 块内函数声明
不要在块内声明一个函数。虽然很多 JS 引擎都支持块内声明函数, 但它不属于 ECMAScript 规范 (见 [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)，第13和14条)。 各个浏览器糟糕的实现相互不兼容, 有些也与未来 ECMAScript 草案相违背。 ECMAScript 只允许在脚本的根语句或函数中声明函数。如果确实需要在块中定义函数，建议使用函数表达式来初始化变量。
```javascript
//不推荐
if (x) {
  function foo() {}
}
```
```javascript
//推荐
var foo = function () {};
if (x) {
  foo();
}
```
#### 封装基本类型
避免封装基本类型。
```javascript
//不推荐
var x = new Boolean(false);
if (x) {
  alert('hi');  // Shows 'hi'.
}
```
#### with
使用 with 改变变量作用域，让你的代码在语义上变得不清晰。
```javascript
//不推荐
with (foo) {
  var x = 3;
  return x;
}
```
#### for-in循环
对 Array 用 for-in 循环有时会出错。 因为它并不是从 0 到 length - 1 进行遍历, 而是所有出现在对象及其原型链的键值， 下面就是一些失败的使用案例。
```javascript
function printArray(arr) {
  for (var key in arr) {
    print(arr[key]);
  }
}

printArray([0,1,2,3]);  // This works.

var a = new Array(10);
printArray(a);  // This is wrong.

a = document.getElementsByTagName('*');
printArray(a);  // This is wrong.

a = [0,1,2,3];
a.buhu = 'wine';
printArray(a);  // This is wrong again.

a = new Array;
a[3] = 3;
printArray(a);  // This is wrong again.
```
#### Array和Object直接量
使用 Array 和 Object 语法，而不使用 Array 和 Object 构造器。
使用 Array 构造器很容易因为传参不恰当导致错误。
```javascript
//不推荐
// Length is 3.
var a1 = new Array(x1, x2, x3);

// Length is 2.
var a2 = new Array(x1, x2);

// If x1 is a number and it is a natural number the length will be x1.
// If x1 is a number but not a natural number this will throw an exception.
// Otherwise the array will have one element with x1 as its value.
var a3 = new Array(x1);

// Length is 0.
var a4 = new Array();
```
虽然 Object 构造器没有上述类似的问题，但鉴于可读性和一致性考虑，最好还是在字面上更清晰地指明。
```javascript
//不推荐
var o = new Object();

var o2 = new Object();
o2.a = 0;
o2.b = 1;
o2.c = 2;
o2['strange key'] = 3;
```
```javascript
//推荐
var o = {};

var o2 = {
  a: 0,
  b: 1,
  c: 2,
  'strange key': 3
};
```
#### 修改内置对象的原型
不要修改内置对象，如 Object.prototype 和 Array.prototype 的原型。

## JavaScript编码风格

### 命名
#### 通用命名
命名规程采用驼峰式的语义化命名方式，如下所示:
`functionNamesLikeThis`, `variableNamesLikeThis`,`ClassNamesLikeThis`, `EnumNamesLikeThis`, `methodNamesLikeThis`, 和 `SYMBOLIC_CONSTANTS_LIKE_THIS`；

## 参考文档
[https://google.github.io/styleguide/javascriptguide.xml](https://google.github.io/styleguide/javascriptguide.xml)

补充：
##JQ编码规范
####需要DOM加载完执行的代码一律放到ready函数里面且页面只写一个文档ready事件的处理程序

```
//推荐
$(function(){ //do something })
```
####关于变量

 1. jQuery类型的变量最好加个$前缀。
 2. 时常将jQuery选择器返回的内容存进变量以便重用

```
var $myShare = $("#myShare");
$myShare.click(function(){...});
```
####关于选择器

 1. 尽量ID选择器。实际运用的是js的document.getElementById()，所以速度较其他选择器快。
 2. 使用类选择器时表指定元素的类型。
 3. ID父亲容器下面查找子元素请用.find()方法。这样做快的原因是通过id选择元素不会使用Sizzle引擎。

```
var $products = $("div.products"); // 慢
var $products = $(".products"); // 快
var $productIds = $("#products div.id");//不推荐
var $productIds = $("#products").find("div.id");//推荐

```
####DOM操作

 1. 使用连接字符串或数组join()，然后再append()。
 2. 不要用缺失的元素
 3. 不要将CSS与jQuery杂揉
```
// 不推荐
var $myList = $("#list");
for(var i = 0; i < 10000; i++){
    $myList.append("<li>"+i+"</li>");
}
// 推荐
var array = []; 
for(var i = 0; i < 10000; i++){
    array[i] = "<li>"+i+"</li>"; 
}
$myList.html(array.join(''));


//不推荐
$("#nosuchthing").slideUp();

//推荐
var $mySelection = $("#nosuchthing");
if ($mySelection.length) {
    $mySelection.slideUp();
}


//不推荐
$("#mydiv").css({'color':red, 'font-weight':'bold'}); 
//推荐
.error {
    color: red;
    font-weight: bold;
}
$("#mydiv").addClass("error"); 
```
###尽量采用链式写法

```
//不推荐
var $item = $("#item");
$item.addClass("error");
$item.show();

//推荐
$item.addClass("error").show();
```
