# 一、初始JavaScript

## 1.1 浏览器执行 JS 的简介

![image-20221102203932732](D:/ProgramFiles/typora/typora-images/image-20221102203932732.png)

## 1.2 变量

### 声明变量

```js
var age; // 声明一个名称为 age的变量
```



![image-20221108204256842](D:/ProgramFiles/typora/typora-images/image-20221108204256842.png)

### 变量的命名规范

![image-20221108204353506](D:/ProgramFiles/typora/typora-images/image-20221108204353506.png)

## 1.3 数据类型

### 简单数据类型

![image-20221108210550749](D:/ProgramFiles/typora/typora-images/image-20221108210550749.png)

![image-20221108211127692](D:/ProgramFiles/typora/typora-images/image-20221108211127692.png)

### typeof 关键字检测变量类型

```js
    console.log(typeof 10);      // number
    console.log(typeof "jfdk"); // string
    console.log(typeof true);   // boolean
    var abc;
    console.log(typeof abc);  // undefined
```



***

**各类型在控制台对应颜色：**

![image-20221108215412293](D:/ProgramFiles/typora/typora-images/image-20221108215412293.png)

***



## 1.4 数据类型转换

### 转换为字符串

| 方式       | 例如                         |
| ---------- | ---------------------------- |
| toString() | int a=2; alert(a.toString()) |
| String()   | int a = 2; alert(String(a))  |
| 使用+      | int a = 2; alert(a + "")     |



### 转换为数字型

| 方式                       | 说明                       | 例如                |
| -------------------------- | -------------------------- | ------------------- |
| parseInt(string)函数       | 将string类型转成整数值型   | parseInt("234")     |
| parseFloat(String)         | 将string类型转成浮点数值型 | parseFloat("23.21") |
| Number(string)强制类型转换 |                            | Number("23")        |
| js隐式转换                 | 利用算术运算转换为数值型   | '12' - 0            |

```js
        console.log(parseInt("2.2f3f"));  // 2
        console.log(parseFloat(23.23));  // 23.23
        console.log(Number("23"));  // 23
        console.log(Number("23f"));  // NaN
        console.log('12' - 0);     // 隐式转换
```



### 转换为boolean类型

| 方式          | 说明                    | 例子         |
| ------------- | ----------------------- | ------------ |
| Boolean()函数 | 其他类型转换boolean类型 | Boolean("1") |

![image-20221108232741235](D:/ProgramFiles/typora/typora-images/image-20221108232741235.png)

***



# 二、运算符

## 2.1 算术运算符

| 运算符 | 实例      |
| ------ | --------- |
| +      | 10+20=30  |
| -      | 10-2=8    |
| *      | 3*2=6     |
| /      | 3/2=1.5   |
| %      | 9 % 2 = 1 |

***

## 2.2 表达式和返回值

**表达式**：是由数字、运算符、变量等以能求得数值的有意义排列方式所得的组合。

例：1 + 1

## 2.3 自增\减运算符

**i++;**

**++i;**

**i--;**

**--i;**

***

## 2.4 比较运算符

![image-20221108234646052](D:/ProgramFiles/typora/typora-images/image-20221108234646052.png)

```js
console.log(18 == "18");//true
console.log(18 === "18");//false
console.log(18 !== "18");//true
```

## 2.5 逻辑运算符

<img src="D:/ProgramFiles/typora/typora-images/image-20221108235346750.png" alt="image-20221108235346750" style="zoom:25%;" />



### 短路运算 &&

如果左边为假则后边就不用算了，如果左边为真，那后边还要算，因为&&要求全部为真才为真。

![image-20221108235959614](D:/ProgramFiles/typora/typora-images/image-20221108235959614.png)

***

### 短路运算 ||

如果左边为假则后边还要算，因为||要求只要有一个为真就为真。

***

## 2.6 赋值运算符

![image-20221109000621148](D:/ProgramFiles/typora/typora-images/image-20221109000621148.png)

## 2.7 运算符优先级

![image-20221109000701448](D:/ProgramFiles/typora/typora-images/image-20221109000701448.png)

***



# 三、分支结构

## 3.1 if 分支结构

**语法**：

```js
if(true/false){
    代码块;
}
//========================================
if(true){
    代码块;
} else{
    其他代码块;
}
//========================================
if(true){
    代码块;
} else if(false){
    代码块;
} else {
    其他代码块;
}
```

***

## 3.2 三元运算符

```js
var b = 1 > 2? "1大于2":"2大于1";
alert(b);
```



## 3.3 switch分支语句

```js
switch(值){
    case v1:console.log(v1);break;
    case v2:console.log(v2);break;
    default:console.log()
}

//注意事项：
// 1. 我们开发里面 表达式我们经常写成变量
// 2. 我们num 的值 和 case 里面的值相匹配的时候是 全等   必须是值和数据类型一致才可以 num === 1
// 3. break 如果当前的case里面没有break 则不会退出switch 是继续执行下一个case
```



# 四、循环结构

## 4.1 for循环

```js
for(var i = 1; i < n; i++){
    console.log(i);
}

// for循环执行顺序：
// 1. 首先执行里面的计数器变量  var i = 1 .但是这句话在for 里面只执行一次  index
// 2. 去 i <= 100 来判断是否满足条件， 如果满足条件  就去执行 循环体  不满足条件退出循环 
// 3. 最后去执行 i++   i++是单独写的代码 递增  第一轮结束 
// 4. 接着去执行 i <= 100 如果满足条件  就去执行 循环体  不满足条件退出循环   第二轮
```

## 4.2 while循环

```js
while(条件表达式){
    //循环体;
}

//===================================
do{
    //循环体;
}while(条件表达式);


// continue  和 break
```



# 五、数组

## 5.1 创建数组

**使用`new`关键字创建**:

```js
var arrs = new Array();

// Array() 的大写
```

**使用字面量创建数组**

```js
var arrs = [];

var person = ["小李","小白","小黑"];
```





## 5.2 访问数组中的元素

**数组的下标从 `0` 开始**

```js
var arr = [];
var firstElement = arr[0]; // 数组的第一个元素
```



**数组的长度 `length`属性**

```js
var arr = [1,2,3];
var len = arr.length;
```



## 5.3 数组中新增元素

###  1. 通过修改 `length`属性

![image-20221109193316395](D:/ProgramFiles/typora/typora-images/image-20221109193316395.png)

***

### 2. 在数组末尾追加

![image-20221109193749290](D:/ProgramFiles/typora/typora-images/image-20221109193749290.png)

***



## 5.4 检测是否为数组

1. `instanceof` 运算符

```js
var arr = [];
console.log(arr instanceof Array); // true
```

2. Array.isArray()方法:

```js
var arr = [];
console.log(Array.isArray(arr))// true
```



## 5.5 数组的添加和删除

**末尾添加**：

```js
var arr = [];
arr.push(4); // push()返回值是数组的长度

```

**开头添加**：

```js
var arr = []
arr.unshift();
```

**删除最后一个元素**：

```js
var arr = [];
arr.pop();
```

**删除第一个元素**：

```js
var arr = [];
arr.shift();
```

![image-20221110153431056](D:/ProgramFiles/typora/typora-images/image-20221110153431056.png)



**翻转数组**：

```js
var arr = [1,2,3];
arr.reverse();// 翻转数组
```

**获取数组索引**：

```js
var arr = [1,2,3];
arr.indexOf(1)// 搜索元素1第一次出现的索引
arr.lastIndexOf(1);//搜索元素1最后一次出现的索引
```

## 5.6 数组转换为字符串

**toString() 和 join(分割符)**

```js
var arr = ["1",3,2];
console.log(arr.join("%"));   // 1%3%2
console.log(arr.toString()); // 1,3,2
```



**数组其他几个常用方法**：

![image-20221110160849709](D:/ProgramFiles/typora/typora-images/image-20221110160849709.png)





# 六、函数

## 函数的使用

**声明函数**：

```js
//方式1：
function 函数名(){   // 命名函数
    //函数体;
}
//================================
 
//方式2：  // 匿名函数
var 变量名 = function(){
    //函数体;
}
```

***函数不调用不执行***

***



## 函数的参数

```js
function fun(var1, var2){
    
}


function fun1(...var1){ // ...var1可变长参数
    
}
```



***函数形参和裸骑个数不匹配问题***

![image-20221109203823380](D:/ProgramFiles/typora/typora-images/image-20221109203823380.png)

***

***函数没有返回值***

```js
function fun(){
    return;
}
console.log(fun());// undefined

//===============================

function fun(){

}
console.log(fun()); // undefined
```



## arguments的使用

**arguments是什么？？**

![image-20221109205315910](D:/ProgramFiles/typora/typora-images/image-20221109205315910.png)

***

![image-20221109205805311](D:/ProgramFiles/typora/typora-images/image-20221109205805311.png)

***



# 七、作用域

![image-20221109210932155](D:/ProgramFiles/typora/typora-images/image-20221109210932155.png)



**局部变量和全局变量**：

![image-20221109211438735](D:/ProgramFiles/typora/typora-images/image-20221109211438735.png)



***



# 八、预解析

**变量预解析和函数预解析**：



**注意变量赋值：**

```js
var a = b = c = 9;
//相当于 var a = 9; b = 9; c = 9;


function fun(){
    var a = b = c = 9;// var a = 9; b = 9; c = 9; 那么 b 和 c 是全局变量
}
```



# 九、js对象

## 9.1 创建对象的三种方式

### 方式1：利用对象字面量

```js
var obj = {}; // 创建了一个空的对象

var person = {
    uname:'张三',
    age: 18,
    sex: '男',
    say: function(){
        //
    }
}
```

**使用对象的方式**：

```js
// 使用对象
方式1：person.属性名
方式2：person[属性名]
```

![image-20221109223154490](D:/ProgramFiles/typora/typora-images/image-20221109223154490.png)







### 方式2：利用 new Object()

```js
var obj = new Object();//创建了一个空的对象;
obj.uname = "老李";
obj.sex = "男";
obj.age = 99;
obj.say = function(){
	console.log("hi")
}
```



### 方式3：利用构造函数创建对象

**构造函语法格式**：

```js
function 构造函数名(){
    this.属性 = 值;
    this.方法 = function(){
        
    }
}

new 构造函数名();

// 1。构造函数名首字母大写
```

***

### new 关键字执行过程

1. new  构造函数可以在内存中创建了一个空的对象
2. this 就会指向刚才创建的空对象
3. 执行构造函数里面的代码，给这个空对象添加属性和方法
4. 返回这个对象



### for in 循环语句

for... in 语句用于对数组或对象的属性进行循环操作。

```js
for(变量 in 对象){
    
}

//===============================

var obj = new Object();//创建了一个空的迹象对象;
obj.uname = "老李";
obj.sex = "男";
obj.age = 99;
obj.say = function () {
    console.log("hi")
}
for(v in obj){ // v 指的是对象的属性名或方法名
    console.log(obj[v]);
}
```





# 十、内置对象

查阅常用的方法：

[MDN Web Docs (mozilla.org)](https://developer.mozilla.org/zh-CN/)

***



## Math对象常用的方法

1. Math.PI    圆周率

2. Math.floor() 向下取整

   ```js
   console.log(Math.floor(2.5)); // 2
   ```

3. Math.ceil() 向上取整

   ```js
   console.log(Math.ceil(2.2)); // 3
   ```

4. Math.round() 四舍五入

   ```js
   console.log(Math.round(-2.5)); // -2
   console.log(Math.round(-2.2)); // -2
   console.log(Math.round(2.5)); // 3
   console.log(Math.round(2.3)); // 2
   ```

5. Math.abs()    取绝对值

6. Math.max()    Math.min()    最大值和最小值

7. Math.random()   随机数方法

   ```js
   
   ```



## Date日期对象的使用

[Date - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)

***



# 十一、基本包装类型

![image-20221110161259262](D:/ProgramFiles/typora/typora-images/image-20221110161259262.png)

***

# 十二、字符串对象

## 根据字符返回位置

```js
var str = "民生东路";
console.log(str.indexOf("民"));
console.log(str.lastIndexOf("路"))
```



## 字符串常见操作

![image-20221110162438831](D:/ProgramFiles/typora/typora-images/image-20221110162438831.png)

***



# 十三、简单数据类型和复杂数据类型



## 简单类型和复杂类型

![image-20221110163353902](D:/ProgramFiles/typora/typora-images/image-20221110163353902.png)

***

## 堆和栈

简单类型放在栈，复杂类型放在堆。



# 十四、Web Apis

## 14.1 获取元素

### 根据元素id获取

```js
<div id="id名字"></div>
document.getElementById("id名字")
```



![image-20221110213747220](D:/ProgramFiles/typora/typora-images/image-20221110213747220.png)



**获取`body`和`html`标签**：

![image-20221110223155494](D:/ProgramFiles/typora/typora-images/image-20221110223155494.png)



## 14.2 事件基础



**事件三要素**：

![image-20221110223630424](D:/ProgramFiles/typora/typora-images/image-20221110223630424.png)



**事件执行的步骤**：

1. 获取事件源
2. 注册事件
3. 添加事件处理程序

**常见的鼠标事件**：

![image-20221110224103282](D:/ProgramFiles/typora/typora-images/image-20221110224103282.png)



## 14.3 修改元素内容

**innerText和innerHtml的区别**：

innerHtml识别 html 标签，而innerText不识别。



## 14.4 修改元素的属性

![image-20221110231940133](D:/ProgramFiles/typora/typora-images/image-20221110231940133.png)



**修改输入框的属性**

![image-20221110233309034](D:/ProgramFiles/typora/typora-images/image-20221110233309034.png)



**操作样式属性**：

![image-20221113091855254](D:/ProgramFiles/typora/typora-images/image-20221113091855254.png)





## 14.5 获取元素自定义属性

![image-20221113171935887](D:/ProgramFiles/typora/typora-images/image-20221113171935887.png)

***

**设置自定义属性值**：

`element.setAttribute("属性","值");`

**移除自定义属性：**

`element.removeAttribute('属性')`



## 14.6 H5自定义属性

![image-20221113190814936](D:/ProgramFiles/typora/typora-images/image-20221113190814936.png)

***

![](D:/ProgramFiles/typora/typora-images/image-20221113191210966.png)





# 十五、节点操作

**节点描述**：

![image-20221113192110574](D:/ProgramFiles/typora/typora-images/image-20221113192110574.png)

***

**节点层级**：

![image-20221113192216023](D:/ProgramFiles/typora/typora-images/image-20221113192216023.png)

**兄弟节点**：

![](D:/ProgramFiles/typora/typora-images/image-20221113194541312.png)



**创建节点**：

![](D:/ProgramFiles/typora/typora-images/image-20221113210931583.png)

**添加节点**：

![image-20221113194812453](D:/ProgramFiles/typora/typora-images/image-20221113194812453.png)

![image-20221113195114888](D:/ProgramFiles/typora/typora-images/image-20221113195114888.png)

![image-20221113210348687](D:/ProgramFiles/typora/typora-images/image-20221113210348687.png)

***









**删除节点**：

![image-20221113201505848](D:/ProgramFiles/typora/typora-images/image-20221113201505848.png)



**复制节点**：

![image-20221113202114527](D:/ProgramFiles/typora/typora-images/image-20221113202114527.png)



# 十六、事件高级



## 16.1 注册事件的两种方式

![image-20221113211648793](D:/ProgramFiles/typora/typora-images/image-20221113211648793.png)

![image-20221113211738281](D:/ProgramFiles/typora/typora-images/image-20221113211738281.png)

![image-20221113211832344](D:/ProgramFiles/typora/typora-images/image-20221113211832344.png)



## 删除事件



![image-20221113220711813](D:/ProgramFiles/typora/typora-images/image-20221113220711813.png)



## DOM事件流

![](D:/ProgramFiles/typora/typora-images/image-20221113221256696.png)

```js
// dom 事件流 三个阶段
// 1. JS 代码中只能执行捕获或者冒泡其中的一个阶段。
// 2. onclick 和 attachEvent（ie） 只能得到冒泡阶段。
// 3. 捕获阶段 如果addEventListener 第三个参数是 true 那么则处于捕获阶段  document -> html -> body -> father -> son
// var son = document.querySelector('.son');
// son.addEventListener('click', function() {
//     alert('son');
// }, true);
// var father = document.querySelector('.father');
// father.addEventListener('click', function() {
//     alert('father');
// }, true);
// 4. 冒泡阶段 如果addEventListener 第三个参数是 false 或者 省略 那么则处于冒泡阶段  son -> father ->body -> html -> document
```

![image-20221113223826991](D:/ProgramFiles/typora/typora-images/image-20221113223826991.png)



## 事件对象

```js
        div.onclick=(even)=>{
            console.log(even);
        }
```



## e.target和this的区别

e.target返回的是触发事件的对象，this返回的是绑定事件的对象。

![image-20221113230357920](D:/ProgramFiles/typora/typora-images/image-20221113230357920.png)



## 事件对象常见属性和方法

![image-20221113230803513](D:/ProgramFiles/typora/typora-images/image-20221113230803513.png)

## 阻止事件冒泡

**两种方式**：

1. 使用事件对象的`stopPropagation()方法`
2. 使用事件对象的`cancelBubble属性`



## 事件委托

**事件委托的原理**：

![image-20221113232003747](D:/ProgramFiles/typora/typora-images/image-20221113232003747.png)



## 常见的鼠标事件

![image-20221113232802294](D:/ProgramFiles/typora/typora-images/image-20221113232802294.png)



**禁止鼠标右键菜单**:

![image-20221113233340602](D:/ProgramFiles/typora/typora-images/image-20221113233340602.png)



**禁止鼠标选中**：

`"selectstart"`



**鼠标事件对象**：

![image-20221113234146868](D:/ProgramFiles/typora/typora-images/image-20221113234146868.png)



**常用的键盘事件**：

![image-20221114000438217](D:/ProgramFiles/typora/typora-images/image-20221114000438217.png)



**三个事件的执行顺序  keydown -- keypress -- keyup**

![image-20221114122642115](D:/ProgramFiles/typora/typora-images/image-20221114122642115.png)



# 十七、BOM

![image-20221114140918230](D:/ProgramFiles/typora/typora-images/image-20221114140918230.png)

![image-20221114140930022](D:/ProgramFiles/typora/typora-images/image-20221114140930022.png)



**BOM的构成**：

![image-20221114141054277](D:/ProgramFiles/typora/typora-images/image-20221114141054277.png)

![image-20221114141453878](D:/ProgramFiles/typora/typora-images/image-20221114141453878.png)

## 页面加载事件

![image-20221114141607830](D:/ProgramFiles/typora/typora-images/image-20221114141607830.png)

![image-20221114142010881](D:/ProgramFiles/typora/typora-images/image-20221114142010881.png)



## 调整窗口大小事件

![image-20221114142134544](D:/ProgramFiles/typora/typora-images/image-20221114142134544.png)



```js
window.innerWith//窗口宽度
window.innerHeight//窗口高度
```



## 定时器之setTimeout

![image-20221114142521571](D:/ProgramFiles/typora/typora-images/image-20221114142521571.png)



**停止setTimeout**:

```js
window.clearTimeout()
```

![image-20221114143535143](D:/ProgramFiles/typora/typora-images/image-20221114143535143.png)



**定时器之 setInterval()**:

![image-20221114143739818](D:/ProgramFiles/typora/typora-images/image-20221114143739818.png)



**清除定时器setInterval**:

![image-20221114144336948](D:/ProgramFiles/typora/typora-images/image-20221114144336948.png)



## js同步与异步



![image-20221114150157432](D:/ProgramFiles/typora/typora-images/image-20221114150157432.png)



![image-20221114150620921](D:/ProgramFiles/typora/typora-images/image-20221114150620921.png)



## location对象

**location对象的属性**：

![image-20221114151408549](D:/ProgramFiles/typora/typora-images/image-20221114151408549.png)



**location对象的方法**：

![image-20221114152230973](D:/ProgramFiles/typora/typora-images/image-20221114152230973.png)



**navigator对象**：

![image-20221114152820913](D:/ProgramFiles/typora/typora-images/image-20221114152820913.png)



**history对象**：

![image-20221114153040466](D:/ProgramFiles/typora/typora-images/image-20221114153040466.png)



## 网页特效

### offset

![image-20221114160343873](D:/ProgramFiles/typora/typora-images/image-20221114160343873.png)

**offset和style的区别**:

![image-20221114161119865](D:/ProgramFiles/typora/typora-images/image-20221114161119865.png)



### client

![image-20221114162153191](D:/ProgramFiles/typora/typora-images/image-20221114162153191.png)

### 立即执行函数

```js
(function(){})() //最后一个括号可以看作是调用函数
或者
(function(){}())

/**
不需要调用,立马能够执行的函数;
独立创建了一个作用哉;
*/
```



### scroll

![image-20221114163635973](D:/ProgramFiles/typora/typora-images/image-20221114163635973.png)

### mouseover和mouseenter的区别

![image-20221114164924458](D:/ProgramFiles/typora/typora-images/image-20221114164924458.png)

## 动画原理

![image-20221114165046029](D:/ProgramFiles/typora/typora-images/image-20221114165046029.png)



# 十八、sessionStorage和localStorage

![image-20221114193658279](D:/ProgramFiles/typora/typora-images/image-20221114193658279.png)



# 十九、ES6特性

## let关键字

```js
let i = 2;
//1. let i = 2; let 声明的变量不能重复声明

//2. 块级作用域(块中声明的let变量，不能在块外面作用)
// {
//     let m = 2;
// }
// console.log(m);  //Uncaught ReferenceError: m is not defined

//3. let声明的变量没有“变量提升”
// console.log(m); // Cannot access 'm' before initialization
// let m = 3;
```

## const关键字

```js
// 1. const声明的变量表示常量，声明时必须赋值,且常量变量一般大写
// const i;//报错

// 2. const声明的变量不能修改
// const name = "小李";
// name = "小王";
// console.log(name);  // Assignment to constant variable.

// 3. 块级作用域(外面不能访问)
// {
//     const age = 28;
// }
// console.log(age);// age is not defined

//4. const修改的数组、对象，可以修改其中的值。
```

## 变量的结构赋值

```js
//数组的结构赋值
// const name = ["小王", "小李"];
// let [xiao] = name;  // 
// console.log(xiao); // "小王"

//对象的结构赋值
// const person = {
//     name:'小张',
//     age:18,
//     sex:"男"
// }
// let {name,age}= person;  // {name,age}必须和对象的属性名一样
// console.log(name,age);
```

## 模板字符串

```js
// '' 换行的话需要使用 + 拼接
// let str = 'jfs    fkj \njfdk'+
// 'jfkdls'
// console.log(str);


// 模板字符串 ``可以直接换行
// let str = `lasfd
// ls
// aa`
// console.log(str);

//使用 ${}进行拼接
// let name = "齐天大圣";
// let per = `${name} 是孙悟空`;
// console.log(per); // 齐天大圣 是孙悟空
```



## 对象的简化写法

```js
// 可以直接将变量和函数写在对象的属性和方法
let name = "小张";
let fun = function(){
    console.log("fun");
}
let obj = {
    name,
    fun,
    say(){ // 省去 function
        console.log("say");
    }
}

console.log(obj);
obj.fun()
obj.say()
```

## 箭头函数定义及声明特点

```js
// //1. this 是静态的，始终指向函数声明时所在的作用域下的this
// function getName(){
//     console.log(this.name);
// }
// let getName1 = ()=>{
//     console.log(this.name);
// }
// window.name = "小张";
// const per = {
//     name: '小李'
// }
// // getName();//小张
// // getName1()//小张

// //call方法调用
// getName.call(per); // 小李
// getName1.call(per);//  小张

// 2. 不能作为构造函数实例化对象
// let person = (name,age)=>{
//     this.name = name;
//     this.age = age;
// }
// let he = new person("小张",18);
// console.log(he);//person is not a constructor

// 3. 不能使用 arguments 变量
// let fun = ()=>{
//     console.log(arguments);
// }
// fun(1,2)// arguments is not defined
```



## 函数的参数默认值设置

```js
// 如果参数 c 没有传入，则默认值为1
// let fun=function(a,b,c = 1){
//     return a + b + c;
// }
// console.log(fun(1,2));

// function fun({host = "localhost",user,password,port}){
//     console.log(host, user, password, port);//localhost root root 1280
// }

// fun({
//     // host:'172.23.11.22',
//     user:'root',
//     password:'root',
//     port:1280
// })
```

## 扩展运算符

![image-20221117220950058](D:/ProgramFiles/typora/typora-images/image-20221117220950058.png)



## Symbol的介绍与创建

![image-20221117221058588](D:/ProgramFiles/typora/typora-images/image-20221117221058588.png)

![image-20221117221337825](D:/ProgramFiles/typora/typora-images/image-20221117221337825.png)



**Symbol的使用**：

![image-20221117221707999](D:/ProgramFiles/typora/typora-images/image-20221117221707999.png)





![image-20221117221842207](D:/ProgramFiles/typora/typora-images/image-20221117221842207.png)



## 迭代器

![image-20221117222311459](D:/ProgramFiles/typora/typora-images/image-20221117222311459.png)

![image-20221117222632563](D:/ProgramFiles/typora/typora-images/image-20221117222632563.png)



## 生成器函数

![image-20221117224037076](D:/ProgramFiles/typora/typora-images/image-20221117224037076.png)





# 面向对象

## 静态成员和实例成员



## 原型链



![image-20221118153835700](D:/ProgramFiles/typora/typora-images/image-20221118153835700.png)



## 类的本质 

![image-20221118163718858](D:/ProgramFiles/typora/typora-images/image-20221118163718858.png)





## 改变函数内部this指向

### 1. call方法

![image-20221118192312675](D:/ProgramFiles/typora/typora-images/image-20221118192312675.png)

![image-20221118192538242](D:/ProgramFiles/typora/typora-images/image-20221118192538242.png)

### 2. apply方法

![image-20221118193003431](D:/ProgramFiles/typora/typora-images/image-20221118193003431.png)

### 3. bind方法

![image-20221118193722774](D:/ProgramFiles/typora/typora-images/image-20221118193722774.png)





## 闭包

![](D:/ProgramFiles/typora/typora-images/image-20221118200439438.png)















































