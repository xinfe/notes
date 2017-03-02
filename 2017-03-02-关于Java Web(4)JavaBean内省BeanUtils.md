*author : xinfe*   
*date : 2017-03-02*
***
# ����Java Web(4)JavaBean����ʡ��BeanUtils


### һ��javaBean 

javaBean�������java�࣬�����ڣ�  
- ������ǹ����ġ������ 
- ������һ��Ĭ�ϵ��޲ι��캯��
- �ֶ���˽�е�
- ����������һ���Ĺ���getXxx��setXxx��
	
������
```
public class User
{
    private String name;					//˽��
    private int age;						//˽��
	private Date birthday;					//˽��	
	
    public String getName()					//�������
    {
        return name;
    }
	
    public void setName(String name)		//��������
    {
        this.name = name;
    }
	
    public int getAge()						//��ȡ����
    {
        return age;
    }
	
	public void setAge(int age)				//��������
    {
        this.age = age;
    }
	
	public Date getBirthday()				//��ȡ����
    {
        return birthday;
    }
	
    public void setBirthday(Date birthday)	//��������
    {
        this.birthday = birthday;
    }

}
```




### ������ʡ(Introspector)
  
��ʡ��Wiki���������ͣ�  
> �ڼ������ѧ�У���ʡ��ָ���������������ʱ���������͵�һ��������ͨ��Ҳ���Գ�������ʱ���ͼ�顣��Ӧ�ý���ʡ�ͷ��������
> �������ʡ���������һ������ָ���������������ʱ���Է��ʡ������޸�������״̬����Ϊ��һ��������

�����Ǻ���⣬��ʱֻ֪������������javaBean�����Եġ�  
������þ�������ʾ��һ�£�  
```
PropertyDescriptor pd = new PropertyDescriptor("name",User.class);	//�½�һ����������������(����һ������������������beanClass)
Method method = pd.getWriteMethod();								//��ø����Ե�д����(��setXXX����)
User user = new User();
method.invoke(user,"xinfe");										//���ø÷���(����һ�����󣬲������������ֵ)

method = pd.getReadMethod();										//��ø����ԵĶ�����(��getXXX����)
System.out.println(method.invoke(user,null));						//��ӡ���Ϊxinfe
```

����ʡ��ص�����Ҫ�У�Introspector,BeanInfo,PropertyDescriptor�ȡ�





### ����BeanUtils
������ʡ�Ƚϸ��ӣ����Կ������������jar��BeanUtils��������javabean�����ԡ�    
��jar������Apache�����ģ�����Ҫcommons-logging.jar��֧�֡�  
����ʹ��BeanUtils����Ҫ������jar����Add to Build Path��  
```
User user = new User();

String name = BeanUtils.getProperty(user,"name");		//static String 	getProperty(Object bean, String name)


String age = "21";										//��������ݴӱ��л�ã�����ΪString
BeanUtils.setProperty(user, "age", age);				//static void 	setProperty(Object bean, String name, Object value)
														//ֻ֧�ֻ����������͵�ת��


String birthday = "1996-01-01";							//Ҫת����Date���ͣ���֧�֣���Ҫע��ת����
DateConverter converter = new DateConverter();
converter.setPatterns(new String[]{"yyyy-MM-dd","yyyyMMdd","MM/dd/yyyy"});
ConvertUtils.register(converter, Date.class);			//static void 	register(Converter converter, Class<?> clazz)
BeanUtils.setProperty(user, "birthday", birthday);


Map map = new HashMap();
map.put("name","xinfe");
map.put("age","21");
BeanUtils.populate(user,map);							//static void 	populate(Object bean, Map<String,? extends Object> properties)
```





### �ο�����
[Andye - javabean�Լ���ʡ�������](http://www.cnblogs.com/yejiurui/archive/2012/10/06/2712693.html)   
[zhengxinzhi's blog - Java��ʡ���](http://blog.csdn.net/u014394715/article/details/51217821)   
[BeanUtils API](http://commons.apache.org/proper/commons-beanutils/javadocs/v1.9.3/apidocs/index.html)


***