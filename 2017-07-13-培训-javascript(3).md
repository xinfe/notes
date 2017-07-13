*author : xinfe*    
*date : 2017-07-13*   
***
# javascript(3)

1. 函数search实现在一个已排序的数字类型数组中查找指定数字的功能。     
    search函数语法为       
    var idx = search(arr, dst);           
    使用举例如下：           
    var arr = [1, 2, 4, 6, 7, 9, 19,20, 30, 40, 45, 47]；           
    var idx = search(arr, 45)； // 执行后idx值等于10            
    请给出函数search的3种代码实现，             
    第一种可以使用Array的内置方法，第二种要求不能使用Array的内置方法，第三种尝试使用二分法查询。         
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

        function search1(arr,dst){
            if(arr instanceof Array && typeof(dst)==="number"){
                return arr.indexOf(dst);
            }
            else{
                alert("请传入正确的参数类型");
            }
        }

        function search2(arr,dst){
            if(arr instanceof Array && typeof(dst)==="number"){
                for (i in arr){
                    if (arr[i]===dst){
                        return i;
                    }
                }
            }
            else{
                alert("请传入正确的参数类型");
            }
        }

        function search3(arr,dst){
            if(arr instanceof Array && typeof(dst)==="number"){
                var low=0;
                var high=arr.length;
                var mid;
                while(low<high){
                    mid=Math.floor((low+high)/2);
                    if(arr[mid]===dst){
                        return mid;
                    }
                    if(arr[mid]>dst){
                        high=mid;
                    }else{
                        low=mid;
                    }
                }
            }
            else{
                alert("请传入正确的参数类型");
            }
        }

        var arr = [1, 2, 4, 6, 7, 9, 19,20, 30, 40, 45, 47];

        var idx = search1(arr, 45); // 执行后idx值等于10
        console.log("方法一："+idx);

        var idx = search2(arr, 45); // 执行后idx值等于10
        console.log("方法二："+idx);
        
        var idx = search3(arr, 45); // 执行后idx值等于10
        console.log("方法三："+idx);

        </script>
    </body>
    </html>
    ```
    ![1.PNG](https://i.loli.net/2017/07/13/5967798b39c9a.png)           

2. 函数parseQuery用于解析url查询参数。        
    语法如下：          
    var obj = parseQuery(query)           
    query是被解析的查询参数，函数返回解析后的对象。           
    使用范例如下：          
    var jerry = parseQuery("name=jerry&age=1");           
    jerry; 返回值：{name: " jerry ", age: "1"}           
    var tom = parseQuery("name= tom &age=12&gender&");           
    tom; 返回值：{name: "tom", age: "12", gender: ""}         
    请写出函数parseQuery的实现代码。        
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            function parseQuery(query){
                
                var arrOuter;
                var arrInner;
                var strResult="";

                arrOuter = query.split("&");//按&分割字符串成数组

                for(i in arrOuter){
                    if(arrOuter[i] != ""){//判断按&分割后的数组元素是不是空字符串
                        arrInner = arrOuter[i].split("=");//不是，则按照=分割
                        
                        if(arrInner[1]===undefined){
                            arrInner[1] = "";
                        }

                        strResult = strResult + arrInner[0] + ':"' + arrInner[1].toString().trim() +'",';
                    }
                }
                strResult = "{" + strResult.substring(0,strResult.length-1) + "}";
                console.log(strResult);


            }
            parseQuery("name=jerry&age=1");
            parseQuery("name= tom &age=12&gender&");

        </script>
    </body>
    </html>
    ```
    ![2.PNG](https://i.loli.net/2017/07/13/596779d375062.png)            

3. 实现一个 toCamelStyle 函数，把“aaa-bbb-cc”这种形式的命名转换为“aaaBbbCc”，如： toCamelStyle('margin-left'); // marginLeft。
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            function toCamelStyle(str){

                var arr = str.split('-');
                var result='';
                for(i in arr){
                    if(i>0){
                        arr[i] = arr[i].substring(0,1).toUpperCase() + arr[i].substring(1);
                        //console.log(arr[i]);
                    }
                    result = result + arr[i];
                }
                return result;
            }

            console.log("aaa-bbb-cc  ->  " + toCamelStyle("aaa-bbb-cc"));
            console.log("to-camel-style  ->  " + toCamelStyle("to-camel-style"));

        </script>
    </body>
    </html>
    ```
    ![3.PNG](https://i.loli.net/2017/07/13/59677a0b1cfd7.png)          

 4. 使用闭包的方式实现一个累加函数 addNum，参数为 number 类型，每次返回的结果 = 上一次计算的值 + 传入的值，如： addNum(10); //10 addNum(12); //22 addNum(30); //52
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            function addNum(){
                var a = 0;
                function innerFunc(num){
                    a = a + num;
                    return a;
                } 
                return innerFunc;
            }

            var temp=addNum();
            console.log(temp(10));
            console.log(temp(12));
            console.log(temp(30));

        </script>
    </body>
    </html>
    ```
    ![4.PNG](https://i.loli.net/2017/07/13/59677cc3569f4.png)         

5. 校验邮政编码（由六位组成）
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            function check(){
                var str = document.getElementById("input").value;//获取input的值
                var regx = /^[0-9][0-9]{5}$/;
                if(regx.exec(str)){
                    alert("是邮政编码");
                }else{
                    alert("不是邮政编码");
                }
                return ;
            }

        </script>

        <p>校验邮政编码,在下方输入邮编：</p>
        <input type="text" name="input" id="input" value="">
        <button onclick="check()">检查</button>

        </script>
    </body>
    </html>
    ```

6. 只能输入5-20个以字母开头、可带数字、“_”、“.”的字串。
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript">

            function check(){
                var str = document.getElementById("input").value;//获取input的值
                var regx = /^[a-zA-Z]{1}([a-zA-Z0-9]|[._]){4,19}$/;
                if(regx.exec(str)){
                    alert("符合要求");
                }else{
                    alert("不符合要求");
                }
                return ;
            }

        </script>

        <p>请输入一个字符串，将检查是否符合要求：只能输入5-20个以字母开头、可带数字、“_”、“.”的字串</p>
        <input type="text" name="input" id="input" value="">
        <button onclick="check()">检查</button>

    </body>
    </html>
    ```

***