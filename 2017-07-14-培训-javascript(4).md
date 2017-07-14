*author : xinfe*    
*date : 2017-07-14*   
***
# javascript(4)

### NaN，Number.NaN，isNaN()，Number.isNaN()的理解         

### 一、NaN与Number.NaN

>The global NaN property is a value representing Not-A-Number.         

>The Number.NaN property represents Not-A-Number. Equivalent of NaN.       

>NaN compares unequal (via ==, !=, ===, and !==) to any other value -- including to another NaN value.  Use Number.isNaN() or isNaN() to most clearly determine whether a value is NaN.  Or perform a self-comparison: NaN, and only NaN, will compare unequal to itself.

- NaN是Number类型的一个特殊的值，表示一种状态，即不是一个数字，Number.NaN的含义和NaN一样。       
- 判断是不是一个数字，不能用===或者==来判断，需要用到函数isNaN()和Number.isNaN().      
- NaN是唯一一个不与自身相等的值。         

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">

        //number 类型还有一个很特殊的值，即NaN，它是用来表示是否属于number 类型的一种状态，而不是一个确切的值。 
        console.log('typeof NaN;        //' + typeof NaN);
        console.log('typeof Number.NaN; //' + typeof Number.NaN);

        console.log('************************************************');

        //NaN 不与自身相等，不与其他任何值相等
        console.log('NaN === NaN;       //' + (NaN === NaN));
        console.log('Number.NaN === NaN;    //' + (Number.NaN === NaN));

    </script>
</body>
</html>
```
结果：        
![NaN1.PNG](https://i.loli.net/2017/07/14/59686392cf3d8.png)           

### 二、isNaN()与Number.isNaN()

>The isNaN() function determines whether a value is NaN or not.          
>true if the given value is NaN; otherwise, false.         

>The Number.isNaN() method determines whether the passed value is NaN and its type is Number.          
>true if the given value is NaN and its type is Number; otherwise, false.         

isNaN()和Number.isNaN()区别在于前者只能判断是不是数字，后者在前者的基础上再增加一个条件，类型是否为Number。        

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">

        //判断是不是数字，需要用到函数isNaN(),Number.isNaN(),不能使用===，==，!==和!=
        console.log('isNaN(NaN);            //' + isNaN(NaN));
        console.log('Number.isNaN(NaN);     //' + Number.isNaN(NaN));

        console.log('isNaN(Number.NaN);     //' + isNaN(Number.NaN));
        console.log('Number.isNaN(Number.NaN);  //' + Number.isNaN(Number.NaN));

        console.log('************************************************');

        console.log('isNaN(1);          //' + isNaN(1));
        console.log('Number.isNaN(1);       //' + Number.isNaN(1));

        console.log('isNaN("1");            //' + isNaN("1"));
        console.log('Number.isNaN("1");     //' + Number.isNaN("1"));

        console.log('isNaN(true);           //' + isNaN(true));
        console.log('Number.isNaN(true);        //' + Number.isNaN(true));
    
        console.log('isNaN(null);           //' + isNaN(null));
        console.log('Number.isNaN(null);        //' + Number.isNaN(null));

        console.log('************************************************');

        console.log('isNaN(undefined);      //'+isNaN(undefined));
        console.log('Number.isNaN(undefined);   //'+Number.isNaN(undefined));
        
        console.log('isNaN("Hello World");      //'+isNaN("Hello World"));
        console.log('Number.isNaN("Hello World");   //'+Number.isNaN("Hello World"));

        console.log('isNaN([1,2,3]);            //'+isNaN([1,2,3]));
        console.log('Number.isNaN([1,2,3]);     //'+Number.isNaN([1,2,3]));

        console.log('isNaN({name:"L",age:1});   //'+isNaN({name:"L",age:1}));
        console.log('Number.isNaN({name:"L",age:1});    //'+Number.isNaN({name:"L",age:1}));


    </script>
</body>
</html>
```
结果：       
![2.PNG](https://i.loli.net/2017/07/14/59686392c4392.png)          



### 参考          
![NaN - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)           
![Number.NaN - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/NaN)            
![为什么NaN不等于自己](http://www.cnblogs.com/onepixel/p/5281796.html)         
![对属性NaN的理解纠正和对Number.isNaN() 、isNaN()方法的辨析](http://www.cnblogs.com/Spring-Rain/p/5722594.html)        

***