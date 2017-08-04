*author : xinfe*    
*date : 2017-07-13*   
***
# javascript(2)

### 常用内置对象    
js的内置对象有很多很多，常用的有以下几个：Array，Date，Function，Json，Math，String等.    

1. Array   
    - push(): 添加元素到数组的末尾
    - pop(): 删除数组末尾的元素
    - shift(): 删除数组最前面（头部）的元素
    - unshift(): 添加到数组的前面（头部）
    - indexOf(): 找到某个元素在数组中的索引
    - splice(): 通过索引删除某个元素
    - slice(): 复制一个数组
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            var arr=['beijing','shanghai','chongqi'];
            console.log("原数组："+arr);

            arr.push('jiangsu');//在数组末尾增加一个元素
            console.log("arr.push('jiangsu');   //"+arr);

            arr.pop();//在数组末尾删除一个元素
            console.log("arr.pop();     //"+arr);

            arr.shift();//在数组开头删除一个元素
            console.log("arr.shift();       //"+arr);

            arr.unshift('beijing');//在数组开头增加一个元素
            console.log("arr.unshift('beijing');    //"+arr);

            //indexOf(): 找到某个元素在数组中的索引
            console.log("arr.indexOf('chongqi');    //"+arr.indexOf('chongqi'));

            arr.splice(1,1);//splice(): 通过索引删除某个元素
            console.log("arr.splice(1,1);   //"+arr);

            arr.splice(1);//splice(): 通过索引删除某个元素
            console.log("arr.splice(1);     //"+arr);

            var array = arr.slice();//复制一个数组
            console.log("var array = arr.slice();//"+array);

        </script>
    </body>
    </html>
    ```
    结果：    
    ![array.PNG](https://i.loli.net/2017/07/13/59672c8e83664.png)     

2. Date    
    - Date.now(): 返回自 1970-1-1 00:00:00  UTC (世界标准时间)至今所经过的毫秒数。
    - Date.parse(): 解析一个表示日期的字符串，并返回从 1970-1-1 00:00:00 所经过的毫秒数。

3. Function    
    创建函数有两种方式，如以下代码，区别也可以从运行结果分辨出来。      
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

        fun1();//调用函数fun1，不会出错
        function fun1(){
            console.log("正在执行函数fun1");
        }
        //fun1();//不会出错


        fun2();//在函数定义前调用，出错
        var fun2=function(){console.log("正在执行函数fun2");};
        //fun2();//不会出错


        </script>
    </body>
    </html>
    ```
    结果：    
    ![function1.PNG](https://i.loli.net/2017/07/12/59662a1344d53.png)     

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

        //fun1();//调用函数fun1，不会出错
        function fun1(){
            console.log("正在执行函数fun1");
        }
        fun1();


        //fun2();//在函数定义前调用，出错
        var fun2=function(){console.log("正在执行函数fun2");};
        fun2();


        </script>
    </body>
    </html>
    ```
    结果：      
    ![function2.PNG](https://i.loli.net/2017/07/12/59662a1350e51.png)      

4. Json         
    Json对象主要有两个方法：   
    - JSON.parse(): 将一个字符串解析为JSON，可选地转换生成的值及其属性，并返回值。
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            // JSON 语法规则:
            //   数据为 键/值 对。
            //   数据由逗号分隔。
            //   大括号保存对象
            //   方括号保存数组

            var jsonStr = '{"name":"Tom","age":19}';
            var jsonObj = JSON.parse(jsonStr);
            console.log(jsonObj);
            console.log('name:'+jsonObj.name);
            console.log('age:'+jsonObj.age);

            var jsonArrStr = '{"student":[{"name":"Tom","age":19},{"name":"Jack","age":21},{"name":"Rose","age":21}]}';
            var jsonObj = JSON.parse(jsonArrStr);
            console.log(jsonObj);


        </script>
    </body>
    </html>
    ```
    结果：     
    ![JSON.parse.PNG](https://i.loli.net/2017/07/13/59670b504ff7b.png)    

    - JSON.stringify(): 返回与指定值相对应的一个JSON字符串，可选地仅包含某些属性或以用户定义的方式替换属性值。   
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            // JSON 语法规则:
            //   数据为 键/值 对。
            //   数据由逗号分隔。
            //   大括号保存对象
            //   方括号保存数组

            var jsonObj = {"name":"Tom","age":19};
            var jsonStr = JSON.stringify(jsonObj);//将json对象转换成字符串
            console.log(jsonStr);

            var jsonObj = {"stuednt":[{"name":"Tom","age":19},{"name":"Jack","age":21},{"name":"Rose","age":21}]};
            var jsonStr = JSON.stringify(jsonObj);//将json数组转换成字符串
            console.log(jsonStr);

        </script>
    </body>
    </html>
    ```
    结果：    
    ![JSON.stringify.PNG](https://i.loli.net/2017/07/13/59670b506f4e5.png)     

5. Math
    - Math.abs(x)：返回x的绝对值.
    - Math.floor(x)：返回小于x的最大整数.
    - Math.ceil(x)：返回x向上取整后的值.
    - Math.round(x)：返回四舍五入后的整数.
    - Math.random()：返回0到1之间的伪随机数.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            //取绝对值
            console.log("Math.abs(-12);     //"+Math.abs(-12));

            //向下取整
            console.log("Math.floor(3.14);  //"+Math.floor(3.14));
            console.log("Math.floor(-3.14); //"+Math.floor(-3.14));

            //向上取整
            console.log("Math.ceil(3.14);   //"+Math.ceil(3.14));
            console.log("Math.ceil(-3.14);  //"+Math.ceil(-3.14));

            //四舍五入
            console.log("Math.round(3.14);  //"+Math.round(3.14));
            console.log("Math.round(-3.14); //"+Math.round(-3.14));

            //产生[0,1)之间的随机数
            console.log("Math.random();     //"+Math.random());

            //产生[m,n)之间的随机数,m,n由用户确认
            function myRandom(m,n){
                return Math.random()*(n-m)+m;
            }
            //产生[23.1,27.8)之间的随机数
            console.log("myRandom(23.1,27.8);   //"+myRandom(23.1,27.8));

        </script>
    </body>
    </html>
    ```
   结果：     
   ![Math.PNG](https://i.loli.net/2017/07/13/59675c20498d2.png)        

6. String
    - String.prototype.charAt()：返回特定位置的字符。
    - String.prototype.concat()：连接两个字符串文本，并返回一个新的字符串。
    - String.prototype.indexOf()：从字符串对象中返回首个被发现的给定值的索引值，如果没有找到则返回-1。
    - String.prototype.replace()：用新的子串来替换被匹配的子串。**（只替换一处）**。
    - String.prototype.substr()：通过指定字符数返回在指定位置开始的字符串中的字符。**（参数一为下标，参数二为长度,参数一可以为负，参数二可以省略）**
    - String.prototype.substring()：返回在字符串中指定另个下标之间的字符。**（参数一为下标，参数二为下标，包括参数一但不包括参数二，任意负数被当作0，参数二可以省略）**
    - String.prototype.slice()：摘取一个字符串区域，返回一个新的字符串。**（参数一为下标，参数二为下标，返回值包括参数一但不包括参数二，参数二可以省略可以为负）**
    - String.prototype.split()：通过分离字符串成字串，将字符串对象分割成字符串数组。
    - String.prototype.trim()：从字符串的开始和结尾去除空格。
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            //获得字符串的单个字符
            console.log('"Hello World".charAt(1);   //' + "Hello World".charAt(1));
            console.log('"Hello World"[1];      //' + "Hello World"[1]);

            //连接两个字符串文本，并返回一个新的字符串
            console.log('"Hello".concat(" World");  //' + "Hello".concat(" World"));

            //从字符串对象中返回首个被发现的给定值的索引值，如果没有找到则返回-1。
            console.log('Hello World".indexOf("or");    //'+"Hello World".indexOf("or"));

            //用新的子串来替换被匹配的子串
            console.log('"Hello World".replace("o","e");    //'+"Hello World".replace("o","e"));

            console.log("*************************************************************");

            //通过指定字符数返回在指定位置开始的字符串中的字符。
            console.log('"Hello World".substr(3,2); //'+"Hello World".substr(3,2));
            console.log('"Hello World".substr(3);   //'+"Hello World".substr(3));
            console.log('"Hello World".substr(-3,2);    //'+"Hello World".substr(-3,2));

            console.log("*************************************************************");

            //返回在字符串中指定另个下标之间的字符
            console.log('"Hello World".substring(3,5);  //'+"Hello World".substring(3,5));
            console.log('"Hello World".substring(3);    //'+"Hello World".substring(3));
            console.log('"Hello World".substring(3,-1); //'+"Hello World".substring(3,-1));

            console.log("*************************************************************");

            //摘取一个字符串区域，返回一个新的字符串
            console.log('"Hello World".slice(3,5);  //'+"Hello World".slice(3,5));
            console.log('"Hello World".slice(3);        //'+"Hello World".slice(3));
            console.log('"Hello World".slice(3,-1); //'+"Hello World".slice(3,-1));

            console.log("*************************************************************");

            //过分离字符串成字串，将字符串对象分割成字符串数组
            console.log('"Hello World".split(" ");  //'+"Hello World".split(" "));

            //从字符串的开始和结尾去除空格
            console.log('" Hello World ".trim();        //'+" Hello World ".trim());

        </script>
    </body>
    </html>
    ```
    结果：    
    ![String.PNG](https://i.loli.net/2017/07/13/59676c9aed024.png)    

### 参考      
[javascript-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)     

***
