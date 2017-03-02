*author : xinfe*   
*date : 2017-03-02*
***
# 关于Java Web(4)JavaBean、内省、BeanUtils


### 一、javaBean 

javaBean是特殊的java类，特殊在：  
- 类必须是公开的、具体的 
- 必须有一个默认的无参构造函数
- 字段是私有的
- 方法名符合一定的规则（getXxx，setXxx）
	
举例：
```
public class User
{
    private String name;					//私有
    private int age;						//私有
	private Date birthday;					//私有	
	
    public String getName()					//获得姓名
    {
        return name;
    }
	
    public void setName(String name)		//设置姓名
    {
        this.name = name;
    }
	
    public int getAge()						//获取年龄
    {
        return age;
    }
	
	public void setAge(int age)				//设置年龄
    {
        this.age = age;
    }
	
	public Date getBirthday()				//获取生日
    {
        return birthday;
    }
	
    public void setBirthday(Date birthday)	//设置生日
    {
        this.birthday = birthday;
    }

}
```




### 二、内省(Introspector)
  
内省在Wiki上这样解释：  
> 在计算机科学中，内省是指计算机程序在运行时检查对象类型的一种能力，通常也可以称作运行时类型检查。不应该将内省和反射混淆。
> 相对于内省，反射更进一步，是指计算机程序在运行时可以访问、检测和修改它本身状态或行为的一种能力。

还不是很理解，暂时只知道是用来处理javaBean的属性的。  
下面就用具体例子示范一下：  
```
PropertyDescriptor pd = new PropertyDescriptor("name",User.class);	//新建一个属性描述器对象(参数一：属性名，参数二：beanClass)
Method method = pd.getWriteMethod();								//获得该属性的写方法(即setXXX方法)
User user = new User();
method.invoke(user,"xinfe");										//调用该方法(参数一：对象，参数二：传入的值)

method = pd.getReadMethod();										//获得该属性的读方法(即getXXX方法)
System.out.println(method.invoke(user,null));						//打印结果为xinfe
```

与内省相关的类主要有：Introspector,BeanInfo,PropertyDescriptor等。





### 三、BeanUtils
由于内省比较复杂，所以可以引入第三方jar包BeanUtils，来操作javabean的属性。    
此jar包是由Apache开发的，还需要commons-logging.jar的支持。  
所以使用BeanUtils就需要将两个jar包都Add to Build Path。  
```
User user = new User();

String name = BeanUtils.getProperty(user,"name");		//static String 	getProperty(Object bean, String name)


String age = "21";										//假设此数据从表单中获得，类型为String
BeanUtils.setProperty(user, "age", age);				//static void 	setProperty(Object bean, String name, Object value)
														//只支持基本数据类型的转换


String birthday = "1996-01-01";							//要转换成Date类型，不支持，需要注册转换器
DateConverter converter = new DateConverter();
converter.setPatterns(new String[]{"yyyy-MM-dd","yyyyMMdd","MM/dd/yyyy"});
ConvertUtils.register(converter, Date.class);			//static void 	register(Converter converter, Class<?> clazz)
BeanUtils.setProperty(user, "birthday", birthday);


Map map = new HashMap();
map.put("name","xinfe");
map.put("age","21");
BeanUtils.populate(user,map);							//static void 	populate(Object bean, Map<String,? extends Object> properties)
```





### 参考资料
[Andye - javabean以及内省技术详解](http://www.cnblogs.com/yejiurui/archive/2012/10/06/2712693.html)   
[zhengxinzhi's blog - Java内省详解](http://blog.csdn.net/u014394715/article/details/51217821)   
[BeanUtils API](http://commons.apache.org/proper/commons-beanutils/javadocs/v1.9.3/apidocs/index.html)


***