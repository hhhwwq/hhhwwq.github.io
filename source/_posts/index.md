---
title: 原始值与引用值;判断数据类型;类数组与数组;数组的常见API 
date: 2019-10-16 15:36:05
tags: "JavaScript"
---


1.原始值与引用值类型及区别
--------------------------

原始值(primitive value):

> 存储在栈（stack）中的简单数据段，也就是说，它们的值直接存储在变量访问的位置

引用值(reference value):

> 存储在堆（heap）中的对象，也就是说，存储在变量处的值是一个指针（point），指向存储对象的内存处。

-   有哪些类型是原始类型呢？
    -   Undefined：未定义,值只有一个:undefined
    -   Null：空类型,值只有一个:null,一个对象指向为空了,此时可以赋值为null
    -   Boolean：布尔类型(值只有两个,true,false)
    -   Number：数字类型(整数和小数)
    -   String：字符串类型(值一般都是用单引号或者是双引号括起来)
-   有哪些类型是引用类型呢？
    -   Object
    -   Array
    -   Function
-   原始值类型与引用类型的区别？

    （1）值类型：1、占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址，这就是为何**修改引用类型总会影响到其他指向这个地址的引用变量。**

    2、**保存与复制的是值本身**

    3、使用typeof检测数据的类型

    4、基本类型数据是值类型

    （2）引用类型：
    1、占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。）

    2、**保存与复制的是指向对象的一个指针**

    3、使用instanceof检测数据类型

    4、使用new()方法构造出的对象是引用型

2.判断数据类型typeof、instanceof、Object.prototype.toString.call()、constructor
--------------------------------------------------------------------------------

### (1) typeof关键字

    对一个值使用typeof操作符可能会返回下列某个字符串：
    - "undefined"-如果这个值未定义
    - "boolean"-如果这个值是布尔值
    - "string"-如果这个值是字符串
    - "number"-如果这个值是数值
    - "object"-如果这个值是对象或null
    - "function"-如果这个值是函数

    有些时候，typeof操作符会返回一些令人迷惑但技术上正确的值，例如调用typeof null 会返回“object”，因为特殊值null被认为是一个空的对象引用。

### (2) instanceof关键字 

    instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。其意思就是判断对象是否是某一数据类型（如Array）的实例，请重点关注一下是判断<font color=grey>一个对象是否是数据类型的实例。</font>所以有以下例子：

        console.log(2 instanceof Number);           //false
        console.log(true instanceof Boolean);       //false
        console.log('str' instanceof String);       //false
        console.log([] instanceof Array);           //true
        console.log(function(){} instanceof Function); //true
        console.log({} instanceof Object);           //true

    在这里字面量值，2， true ，'str'不是实例，所以判断值为false。眼见为实，看下面的例子：

        console.log(new Number(2) instanceof Number);  //true

    接着说下undefined 和 null   ,说说为什么这两货比较特殊，实际上按理来说，null的所属类就是Null，undefined就是Undefined，然而事实并非如此，浏览器认为null，undefined不是构造器。但是在 typeof 中你可能已经发现了，typeof null的结果是object，typeof undefined的结果是undefined 。这是js发展过程中的重大失误，需要记住。

### (3) Object.prototype.toString.call() 

    使用 Object 对象的原型方法 toString，即使改变对象的原型，也可以显示正确的数据类型。

    var a=Object.prototype.toString;
    console.log(a.call(2));                     //[object Number]
    console.log(a.call(true));                  //[object Boolean]
    console.log(a.call(function(){}));          //[object Function]
    console.log(a.call({}));                    //[object Object]
    console.log(a.call(undefined));             //[object Undefined]
    console.log(a.call(null));                  //[object Null]

### (4) constructor

    console.log((2).constructor === Number);      //true
    console.log((true).constructor === Boolean);  //true
    console.log(('str').constructor === String);   //true
    console.log(([]).constructor === Array);       //true
    console.log((function(){}).constructor === Function); //true
    console.log(({}).constructor === Object);           //true

    用costructor来判断类型看起来是完美的，然而，如果创建一个对象，更改它的原型，这种方式也变得不可靠了。

    function Fn(){};
    Fn.prototype= new Array();
    var f=new Fn();
    console.log(f.constructor === Fn);              //false
    console.log(f.constructor === Array);           //true

    所以最精准的判断方式是第三种Object.prototype.toString.call()。

3.类数组与数组的区别与转换 
---------------------------

-   什么是类数组对象(ArrayLike)

    -   拥有length属性，其它属性（索引）为非负整数(对象中的索引会被当做字符串来处理，这里你可以当做是个非负整数串来理解)
    -   不具有数组所具有的方法

    //类数组示例

    var a = {‘1’:‘gg’,‘2’:‘love’,‘4’:‘meimei’,length:5};

    Array.prototype.join.call(a,‘+’);//‘+gg+love++meimei’

    //非类数组示例

    var c = {‘1’:2}; //没有length属性就不是类数组

javascript中常见的类数组有**arguments对象**和
DOM方法的返回结果。比如 **document.getElementsByTagName()**。 -
类数组和数组的区别

    类数组对象：

    console.log(typeof a);//object 注意：数组也是对象哦

    console.log(a); //  Object {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81} 很明显对象啊

    console.log(a.length); //undefined  区别就在这了  类数组对象没有长度的属性和数组的方法

    console.log(Object.prototype.toString.call(a));//[object Object] 

    数组对象：

    console.log(typeof b);//object

    console.log(b);//  [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]  很明显数组啊 

    console.log(b.length); //8

    console.log(Object.prototype.toString.call(b));//[object Array]

-   类数组转换为数组方法

        Array.prototype.slice.call(arrayLike)

    首先Array.prototype.slice.call(arrayLike)的结果是将arrayLike对象转换成一个Array对象。所以其后面可以直接调用数组具有的方法

        Array.prototype.slice.call(arrayLike).forEach(function(element,index){  //可以随意操作每一个element了 })

    （1）Array.prototype.slice表示数组的原型中的slice方法。**注意这个slice方法返回的是一个Array类型的对象。**

        //slice的内部实现
        Array.prototype.slice = function(start,end){  
            var result = new Array();  
            start = start || 0;  
            end = end || this.length; //this指向调用的对象，当用了call后，能够改变this的指向，也就是指向传进来的对象，这是关键  
            for(var i = start; i < end; i++){  
                result.push(this[i]);  
            }  
            return result;  
        } 

    （2）能调用call的只有方法，所以不能用[].call这种形式，得用[].slice。而call的第一个参数表示真正调用slice的环境变为了arrayLike对象。所以就好像arrayLike也具有了数组的方法。

    -   将数组转换为参数列表（类数组）

    调用apply方法的时候,第一个参数是对象(this), 第二个参数是一个数组集合,
     
    这里就说明apply的一个巧妙用法，可以将一个数组默认的转换为一个参数列表([param1,param2,param3] 转换为 param1,param2,param3)， 这个如果让我们用程序来实现将数组的每一个项,来转换为参数的列表,可能都得费一会功夫,借助apply的这点特性,所以就有了以下高效率的方法。

    （1）Math.max 实现得到数组中最大的一项

    （2）Array.prototype.push 可以实现两个数组合并

4.数组的常见API 
----------------

### 第一组：操作方法 

1.  concat():基于当前数组，创建一个当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组；

    var color2 = [“red”,“green”,“blue”]; var colors2 =
    color.concat(“yellow”,[“black”,“brown”]); alert(colors);
    //red,green,blue alert(colors2); //red,green,blue,yellow,black,brown

2.  slice():基于当前数组中的一个或多个创建一个新数组，可以接受一个或两个参数，要返回项的起始和结束位置。如果参数中有一个负数，则用数组长度加上该数来确定相应的位置。

3.  splice():主要用法是向数组的中部插入项，会改变原数组，是最强大的数组方法。最多指定3个参数，使用这种方法的方式主要有以下3种

    1.  删除：可以删除任意数量的项，指定2个参数，要删除的第一项的位置和要删除的项数。splice(0,2)删除数组的前两项。
    2.  插入：可以向指定位置插入任意数量的项，指定3个参数：起始位置，0(要删除的项数)和要插入的项。splice(2,0,“red”,“green”)会从当前数组的位置2开始插入字符串”red”和”green”。
    3.  替换：可向指定位置插入任意数量的项，且同时删除任意数量的项。splice(2,1,“red”,“green”)会删除当前数组的位置2的项，然后再从位置2开始插入字符串”red”和”green”。

### 第二组：位置方法 

indexOf()和lastIndexOf()。
这两个方法都接受两个参数：要查找的项和(可选的)表示查找起点位置的索引。indexOf()从数组的开头开始向后查找，lastIndexOf()从数组的末尾开始向前查找。

### 第三组：迭代方法

ES5为数组定义了5个迭代方法,这些方法都不会修改数组中包含的值。

每个方法接受两个参数，要在每一项上运行的函数和运行该函数的作用域对象，传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身(item,index,array）。

以下是5个迭代器方法的作用

    (1)every():对数组的每一项运行给定函数，如果数组的每一项都返回true，则返回true。对数组应用该方法，有返回值为true或false 

    (2)some():对数组的每一项运行给定函数，如果数组的任一项都返回true，则返回true。对数组应用该方法，有返回值为true或false 

    (3)filter():对数组的每一项运行给定函数，返回该函数中会返回true的项组成的数组。有返回值，为符合条件的项组成的数组 

    (4)map()::对数组的每一项运行给定函数,有返回每次函数调用的结果组成的数组。 

    (5)forEach():对数组的每一项运行给定函数,这个方法没有返回值。本质上与使用for循环迭代数组是一样的。

### 第四组：归并方法

reduce()和reduceRight()。他们的区别在于从哪头开始遍历数组，除此之外，它们完全相同。

这两个方法都接收两个参数：一个在每一项上调用的函数和(可选)作为归并基础的初始值。

传给reduce()和reduceRight()的函数接收4个参数：前一个值、当前值、项的索引和数组对象。

### 第五组：转换方法 

toLocaleString(),toString()和valueOf()方法。

valueOf()方法返回的是数组本身，而调用toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。toLocaleString()调用的是每一项的toLocaleString()方法，而不是toString()方法。

### 第六组：重排序方法 

reverse()和sort()方法都是用来重排序的方法，需要注意的是对于sort()方法来说，即使数组中的每一项都是数值，sort()方法比较的也是字符串。


