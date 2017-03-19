*author : xinfe*   
*date : 2017-02-27*
***
# 关于Java Web(3)Java的反射机制
### 1. 反射概念
是指在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；
对于任意一个对象，都能够调用它的任意方法和属性；    
反射，通俗点讲，就是可以根据一个类，分解出它的所有成员。    
    
假设我们已经拥有一个类com.xinfe.User，代码如下：   
```java
class User{

	public String name;					//公有属性
	private String psd;					//私有属性
	public static String TAG = "human";			//静态属性
	
	public User(){						//无参构造函数
		this.name = null;
		this.psd = null;
	}
	
	public User(String name,String psd){			//有参构造函数
		this.name = name;
		this.psd = psd;
	}

	public void setName(String name){			//一般方法，无返回值，含参数
		this.name = name;
	}
	
	public String getName(){				//一般方法，有返回值，不含参数
		return name;
	}
	
	private String getPsd(){				//私有方法
		return psd;
	}
	
	public static String getTag(){				//静态方法
		return TAG;
	}
	
}
```
下面我们根据实际例子进行一些操作，便于理解反射的概念和用处。   




### 2. 加载类（三种方法）
```java
Class c1 = Class.forName("com.xinfe.User");	//调用类Class的静态方法forName()，传入需要加载的类的全称，返回该类的字节码。此方法最常用。

Class c2 = new User().getClass();		//调用需要加载的类的一个实例的getClass()方法，返回该类的字节码。

Class c3 = User.class;				//调用需要加载的类的class属性，返回该类的字节码。
```




### 3. 反射类的构造函数
```java
Class c = Class.forName("com.xinfe.User");			//加载类

User user = (User)c.newIntance();				//创建对象（前提是必须有无参构造函数）

Constructor con1 = c.getConstructor(String.class,String.class);	//参数表示调用有参构造函数时需要传入的参数的类型的字节码
User user1 = (User)con1.newInstance("user1","user1");		//参数表示调用有参构造函数时需要传入的实际参数
```
相关API文档：      
![getConstructor.jpg](https://ooo.0o0.ooo/2017/02/27/58b41b4a24382.jpg)





### 4. 反射类的方法
```java
Class c = Class.forName("com.xinfe.User");		//加载类

User user = (User)c.newIntance();			//创建对象

Method method1 = c.getMethod("setName",String.class);	//参数一：方法名（表示调用哪个方法）；参数二：参数类型的字节码
method1.invoke(user,"user2");				//参数一：对象（表示调用该对象的方法）；参数二：实际参数

Method method2 = c.getMethod("getName",null);		//参数表示调用getName()方法，不需要参数
String name = (String)method2.invoke(user,null);	//参数表示调用实例user的getName()方法，不需要参数。强转获得返回值

Method method3 = c.getDeclaredMethod("getPsd",null);	//参数表示调用私有getPsd()方法，不需要参数
method3.setAccessable(true);				//设置该方法的可见性（因为该方法是私有的）破环了安全性
String psd = (String)method3.invoke(user,null);		//参数表示调用实例user的getPsd()方法，不需要参数。强转获得返回值

Method method4 = c.getMethod("getTag",null);		//参数表示调用静态getTag()方法，不需要参数
String tag = (String)method4.invoke(null,null);		//参数表示不需要传入实例(传也行)，不需要参数。强转获得返回值

```
相关API文档：   
![getMethod.jpg](https://ooo.0o0.ooo/2017/02/27/58b425219da13.jpg)



### 5. 反射类的字段
```java
Class c = Class.forName("com.xinfe.User");		//加载类

User user = (User)c.newInstance();			//创建实例

Field field1 = c.getField("name");			//参数表示获取哪一个字段

Object value = field1.get(user);			//获取该字段的值，参数表示具体实例，返回类型为Object
Class type = field1.getType();				//获取该字段的类型,返回类型为Class

if(type.equals(String.class)){				//判断字段的类型是否为String
	String name = (String)value;			//是则强转
}


Field field2 = c.getDeclaredField("psd");		//注意该字段为私有的
field2.setAccessable(true);				//设置可见性
String psd = (String)field2.get(user);			//获取字段值
```
相关API文档：   
![getField.jpg](https://ooo.0o0.ooo/2017/02/27/58b42d631f531.jpg)

### 6. 反射的作用
大家都知道，程序需要先编译才能运行。   
java反射却能实现在运行时期动态加载类，获知它的属性和方法。即那个被加载的类没有事先编译过。   
现在只能理解到这儿，都说反射一般用于框架，等以后遇到了应该还能结合例子深入理解。    


### 参考资料
- [SegmentFault - java反射机制作用](https://segmentfault.com/q/1010000000315618)   
- [CSDN - 反射在JAVA起到的作用](http://bbs.csdn.net/topics/320106718)      
***
