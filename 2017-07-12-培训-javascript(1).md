*author : xinfe*    
*date : 2017-07-12*   
***
# javascript(1)

### 一、基本数据类型    
js的数据类型有7种：undefined，null，number，string，boolean，symbol，（前六种为原始值），object。   

>原始值    
>除 Object 以外的所有类型都是不可变的（值本身无法被改变）。例如，与 C 语言不同，JavaScript 中字符串是不可变的（译注：如，JavaScript 中对字符串的操作一定返回了一个新字符串，原始字符串并没有被改变）。我们称这些类型的值为“原始值”。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">
        // 使用var对变量进行声明
        var a;
        console.log("var a;             //"+ typeof a);

        var b=1;
        console.log("var b=1;           //"+ typeof b);

        var c='string';
        console.log("var c='string';            //"+ typeof c);

        var d=true;
        console.log("var d=true;            //"+ typeof d);

        var e=[1,2,3];
        console.log("var e=[1,2,3]          //"+ typeof e);

        var f={name:'Tom',age:19};
        console.log("var f={name:'Tom',age:19}; //"+ typeof f);

        var g=null;
        console.log("var g=null;            //"+ typeof g);

        var h=function(){};
        console.log("var h=function(){};        //"+ typeof h);

    </script>
</body>
</html>
```
结果：   
![数据类型.PNG](https://ooo.0o0.ooo/2017/07/12/5965e613c49bb.png)    

其中，对于null，数组，对象进行typeof的结果均是object；对function进行typeof的结果是function。

对于为什么typeof null=="object",有解释如下：    
>在 JavaScript 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是0。由于 null 代表的是空指针(大多数平台下值为0x00)，因此，null的类型标签也成为了0，typeof null就错误的返回了"object".
>该现象有待于在ECMAScript 6中被修复 (该提议已被否决). 正确的返回值将成为 typeof null === 'null'.

参考：![typeof - JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)    

### 二、===与==    
- ===：表示比较的两者必须完全相同（值和类型）。先进行类型的比较，如果类型不同，则返回false，否则，再进行值的比较。
- ==：只要值相等就返回true。如果类型不同，（如果可以进行转换的话）会先进行类型的转换。   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">

        var b1 = (1==='1');//符号两边完全一样（类型和值）才返回true
        console.log("var b1 = (1==='1');//"+b1);

        var b2 = (1=='1');//符号两边值一样返回true.先把‘1’转成number类型
        console.log("var b2 = (1=='1');//"+b2);

        console.log("Number('1')="+Number('1'));

    </script>
</body>
</html>
```
结果：    
![===&==.PNG](https://ooo.0o0.ooo/2017/07/12/59660caccae10.png)    


### 参考
![javascript-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)

***