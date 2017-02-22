*author : xinfe*   
*date : 2017-02-22*   
***   
# 关于Java Web(1)回顾Java的部分基础   
### 一、 JDK和JRE    
1. JDK(Java Development Kit)：Java语言的软件开发工具包。其基本组件有多个，主要要知道两个：javac和java。  
javac – 编译器，将源程序转成字节码（将.java文件转为.class文件）;   
java – 运行编译后的java程序（即运行.class文件）;    
有了JDK就可以开发Java应用程序，编译并运行。   

2. JRE(Java Runtime Environment)：Java运行环境。主要包括JVM、java核心类库和支持文件。    
JVM遇到.class的文件就可以直接翻译成它能识别的底层机器语言并进行执行，这就是JVM的机制。类似windows操作系统遇到.exe文件能直接翻译成自己能够识别的语言并执行。  
有了JRE就可以运行编译好的java程序。

### 二、 解释型语言和编译型语言    
1. 解释型语言：   
java就是典型的解释型语言，因为在运行.class文件时，JVM会将字节码转换（解释）成操作系统能够听懂的0101。   

2. 编译型语言：  
比如c和c++，由它们写成的程序被编译之后能被操作系统直接“听懂”。  

### 三、 对于eclipse   
1. 新建的java project，如果选择的编译器版本大于运行版本，则会报错；反之，不会。   
修改编译器版本步骤如下图：   
![1_meitu_1.jpg](https://ooo.0o0.ooo/2017/02/22/58ad4bfc000d4.jpg)   

2. 断点调试   
![断点调试1.png](https://ooo.0o0.ooo/2017/02/22/58ad575fe495d.png)    

3. JUnit   
单元测试是指对软件中最小可测试单元进行检查和验证，最小可测试单元一般就是指程序代码。JUnit就是针对Java的单元测试工具。
	> JUnit常用注解有：     
	> @Before：在任何一个测试执行之前都必须执行的代码；    
	> @After： 在任何一个测试执行之后都必须执行的收尾工作；    
	> @BeforeClass：在所有方法执行之前执行，该方法必须是public和static的；    
	> @AfterClass： 在所有方法执行之后执行，该方法必须是public和static的；    
	> @Test：设置当前方法需要被测试；   
	> @Test(timeout = xxx)：设置当前测试方法在一定时间内运行完，否则返回错误；   
	> @Test(expected = Exception.class)：设置被测试的方法是否有异常抛出。抛出异常类型为：Exception.class；   
	> @Ignore：注释掉一个测试方法或一个类，被注释的方法或类，不会被执行。一般用于忽略一些未完成的方法；    
	> @RunWith(Parameterized. class)：**标注一个测试类**，设置对该测试类使用的Runner为Parameterized. class，不添加@RunWith表示对该测试类使用默认Runner。


### 参考资料：   
[zeko075的专栏 - java的一次编译多次运行机制](http://blog.csdn.net/zeko075/article/details/8565437)   
[华丽的痘痘 - 在Eclipse中使用JUnit4进行单元测试](http://blog.csdn.net/andycpp/article/details/1327147/) 
*** 