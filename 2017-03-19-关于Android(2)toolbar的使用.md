*author : xinfe*   
*date : 2017-03-19*
***
# 关于Android(2)toolbar的使用


### 一、基本使用
1. 使用 toolbar 的 Activity 需要继承 AppCompatActivity。
2. 在 AndroidManifest.xml 中，将 application 标签的 theme 设置为 NoActionBar 。
	```
	<application
		android:theme="@style/Theme.AppCompat.Light.NoActionBar"
    />
	```
	
	或者在 styles.xml 中设置 parent ：   
	![style.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3a423b53f.png)   
	
3. 为 Activity 的布局增加以下内容：
	```
	<android.support.v7.widget.Toolbar
		android:id="@+id/my_toolbar"
		android:layout_width="match_parent"
		android:layout_height="?attr/actionBarSize"
		android:background="?attr/colorPrimary"
		android:elevation="4dp"
		android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
		app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
	```
	
4. 在 Activity 中增加以下代码：
	```
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_my);
		Toolbar myToolbar = (Toolbar) findViewById(R.id.my_toolbar);
		setSupportActionBar(myToolbar);
    }
	```
	
5. 这样便能显示最基础的toolbar。（带有app name，背景颜色为colorPrimary）


### 二、为 toolbar 增加一个返回按钮
1. 首先要有一个 parentActivity （这样按了返回按钮才有去处）。   

2. 在 AndroidManifest.xml 中声明子活动的父活动是谁，如下：   
	![mf.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3ea9970e0.png)     
	
3. 在子活动的 onCreate 方法里设置显示返回按钮：   
	![up.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3f73ec356.png)   
	
4. 如此就实现了返回按钮功能啦。   



### 三、自定义 toolbar （左边一个返回按钮，中间显示标题）

1. 在 layout 中增加一个 toolbar.xml ，增加一个 TextView 。内容如下：  
	![toolbar_xml.PNG](https://ooo.0o0.ooo/2017/03/19/58ce4933cf740.png)   

2. 增加一个布局文件 activity_classification.xml ，引入 toolbar 的布局：   
	![include.PNG](https://ooo.0o0.ooo/2017/03/19/58ce495d2cfb9.png)   

3. 创建一个 BaseToolbarActivity 作为父类,设置不显示默认标题，显示返回按钮;创建一个 toolbar 的帮助类：  
	![baseactivity.PNG](https://ooo.0o0.ooo/2017/03/19/58ce4989ccb44.png)   
	![helper.PNG](https://ooo.0o0.ooo/2017/03/19/58ce44f4d961a.png)   

4. 创建一个继承 BaseToolbarActivity 的子类 ClassificationActivity ，重写方法：   
	![继承的子活动.PNG](https://ooo.0o0.ooo/2017/03/19/58ce49a172b79.png)   

5. 在清单文件中声明（实现在 ClassificationActivity 点击返回按钮回到 MainActivity）：   
	![up.PNG](https://ooo.0o0.ooo/2017/03/19/58ce49bc77006.png)

6. 效果：   
	![home.png](https://ooo.0o0.ooo/2017/03/19/58ce4e0fe52a5.png)   
	![ClassificationActivity.png](https://ooo.0o0.ooo/2017/03/19/58ce4e2c73dda.png)   

### 参考资料
[Android Developers](https://developer.android.com/training/appbar/setting-up.html)   
[Toolbar使用详解](http://www.jianshu.com/p/b3a40a55826e)

***