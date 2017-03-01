*author : xinfe*   
*date : 2017-02-27*
***
# ����Java Web(3)Java�ķ������
### 1.�������
��ָ������״̬�У���������һ���࣬���ܹ�֪���������������Ժͷ�����
��������һ�����󣬶��ܹ������������ⷽ�������ԣ�    
���䣬ͨ�׵㽲�����ǿ��Ը���һ���࣬�ֽ���������г�Ա��    
    
���������Ѿ�ӵ��һ����com.xinfe.User���������£�   
```
class User{

	public String name;						//��������
	private String psd;						//˽������
	public static String TAG = "human";		//��̬����
	
	public User(){							//�޲ι��캯��
		this.name = null;
		this.psd = null;
	}
	
	public User(String name,String psd){	//�вι��캯��
		this.name = name;
		this.psd = psd;
	}

	public void setName(String name){		//һ�㷽�����޷���ֵ��������
		this.name = name;
	}
	
	public String getName(){				//һ�㷽�����з���ֵ����������
		return name;
	}
	
	private String getPsd(){				//˽�з���
		return psd;
	}
	
	public static String getTag(){			//��̬����
		return TAG;
	}
	
}
```
�������Ǹ���ʵ�����ӽ���һЩ������������ⷴ��ĸ�����ô���   




### 2.�����ࣨ���ַ�����
```
Class c1 = Class.forName("com.xinfe.User");	//������Class�ľ�̬����forName()��������Ҫ���ص����ȫ�ƣ����ظ�����ֽ��롣�˷�����á�

Class c2 = new User().getClass();			//������Ҫ���ص����һ��ʵ����getClass()���������ظ�����ֽ��롣

Class c3 = User.class;						//������Ҫ���ص����class���ԣ����ظ�����ֽ��롣
```




### 3.������Ĺ��캯��
```
Class c = Class.forName("com.xinfe.User");						//������

User user = (User)c.newIntance();								//��������ǰ���Ǳ������޲ι��캯����

Constructor con1 = c.getConstructor(String.class,String.class);	//������ʾ�����вι��캯��ʱ��Ҫ����Ĳ��������͵��ֽ���
User user1 = (User)con1.newInstance("user1","user1");			//������ʾ�����вι��캯��ʱ��Ҫ�����ʵ�ʲ���
```
���API�ĵ���      
![getConstructor.jpg](https://ooo.0o0.ooo/2017/02/27/58b41b4a24382.jpg)





### 4.������ķ���
```
Class c = Class.forName("com.xinfe.User");				//������

User user = (User)c.newIntance();						//��������

Method method1 = c.getMethod("setName",String.class);	//����һ������������ʾ�����ĸ������������������������͵��ֽ���
method1.invoke(user,"user2");							//����һ�����󣨱�ʾ���øö���ķ���������������ʵ�ʲ���

Method method2 = c.getMethod("getName",null);			//������ʾ����getName()����������Ҫ����
String name = (String)method2.invoke(user,null);		//������ʾ����ʵ��user��getName()����������Ҫ������ǿת��÷���ֵ

Method method3 = c.getDeclaredMethod("getPsd",null);	//������ʾ����˽��getPsd()����������Ҫ����
method3.setAccessable(true);							//���ø÷����Ŀɼ��ԣ���Ϊ�÷�����˽�еģ��ƻ��˰�ȫ��
String psd = (String)method3.invoke(user,null);			//������ʾ����ʵ��user��getPsd()����������Ҫ������ǿת��÷���ֵ

Method method4 = c.getMethod("getTag",null);			//������ʾ���þ�̬getTag()����������Ҫ����
String tag = (String)method4.invoke(null,null);			//������ʾ����Ҫ����ʵ��(��Ҳ��)������Ҫ������ǿת��÷���ֵ

```
���API�ĵ���   
![getMethod.jpg](https://ooo.0o0.ooo/2017/02/27/58b425219da13.jpg)



### 5.��������ֶ�
```
Class c = Class.forName("com.xinfe.User");		//������

User user = (User)c.newInstance();				//����ʵ��

Field field1 = c.getField("name");				//������ʾ��ȡ��һ���ֶ�

Object value = field1.get(user);				//��ȡ���ֶε�ֵ��������ʾ����ʵ������������ΪObject
Class type = field1.getType();					//��ȡ���ֶε�����,��������ΪClass

if(type.equals(String.class)){					//�ж��ֶε������Ƿ�ΪString
	String name = (String)value;				//����ǿת
}


Field field2 = c.getDeclaredField("psd");		//ע����ֶ�Ϊ˽�е�
field2.setAccessable(true);						//���ÿɼ���
String psd = (String)field2.get(user);			//��ȡ�ֶ�ֵ
```
���API�ĵ���   
![getField.jpg](https://ooo.0o0.ooo/2017/02/27/58b42d631f531.jpg)

### 6. ���������
��Ҷ�֪����������Ҫ�ȱ���������С�   
java����ȴ��ʵ��������ʱ�ڶ�̬�����࣬��֪�������Ժͷ��������Ǹ������ص���û�����ȱ������   
����ֻ����⵽�������˵����һ�����ڿ�ܣ����Ժ�������Ӧ�û��ܽ������������⡣    


### �ο�����
[SegmentFault - java�����������](https://segmentfault.com/q/1010000000315618)   
[CSDN - ������JAVA�𵽵�����](http://bbs.csdn.net/topics/320106718)      
***