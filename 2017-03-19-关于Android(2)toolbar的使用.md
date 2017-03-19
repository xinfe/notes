*author : xinfe*   
*date : 2017-03-19*
***
# ����Android(2)toolbar��ʹ��


### һ������ʹ��
1. ʹ�� toolbar �� Activity ��Ҫ�̳� AppCompatActivity��
2. �� AndroidManifest.xml �У��� application ��ǩ�� theme ����Ϊ NoActionBar ��
	```
	<application
		android:theme="@style/Theme.AppCompat.Light.NoActionBar"
    />
	```
	
	������ styles.xml ������ parent ��   
	![style.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3a423b53f.png)   
	
3. Ϊ Activity �Ĳ��������������ݣ�
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
	
4. �� Activity ���������´��룺
	```
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_my);
		Toolbar myToolbar = (Toolbar) findViewById(R.id.my_toolbar);
		setSupportActionBar(myToolbar);
    }
	```
	
5. ����������ʾ�������toolbar��������app name��������ɫΪcolorPrimary��


### ����Ϊ toolbar ����һ�����ذ�ť
1. ����Ҫ��һ�� parentActivity ���������˷��ذ�ť����ȥ������   

2. �� AndroidManifest.xml �������ӻ�ĸ����˭�����£�   
	![mf.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3ea9970e0.png)     
	
3. ���ӻ�� onCreate ������������ʾ���ذ�ť��   
	![up.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3f73ec356.png)   
	
4. ��˾�ʵ���˷��ذ�ť��������   



### �����Զ��� toolbar �����һ�����ذ�ť���м���ʾ���⣩

1. �� layout ������һ�� toolbar.xml ������һ�� TextView ���������£�  
	![toolbar_xml.PNG](https://ooo.0o0.ooo/2017/03/19/58ce4933cf740.png)   

2. ����һ�������ļ� activity_classification.xml ������ toolbar �Ĳ��֣�   
	![include.PNG](https://ooo.0o0.ooo/2017/03/19/58ce495d2cfb9.png)   

3. ����һ�� BaseToolbarActivity ��Ϊ����,���ò���ʾĬ�ϱ��⣬��ʾ���ذ�ť;����һ�� toolbar �İ����ࣺ  
	![baseactivity.PNG](https://ooo.0o0.ooo/2017/03/19/58ce4989ccb44.png)   
	![helper.PNG](https://ooo.0o0.ooo/2017/03/19/58ce44f4d961a.png)   

4. ����һ���̳� BaseToolbarActivity ������ ClassificationActivity ����д������   
	![�̳е��ӻ.PNG](https://ooo.0o0.ooo/2017/03/19/58ce49a172b79.png)   

5. ���嵥�ļ���������ʵ���� ClassificationActivity ������ذ�ť�ص� MainActivity����   
	![up.PNG](https://ooo.0o0.ooo/2017/03/19/58ce49bc77006.png)

6. Ч����   
	![home.png](https://ooo.0o0.ooo/2017/03/19/58ce4e0fe52a5.png)   
	![ClassificationActivity.png](https://ooo.0o0.ooo/2017/03/19/58ce4e2c73dda.png)   

### �ο�����
[Android Developers](https://developer.android.com/training/appbar/setting-up.html)   
[Toolbarʹ�����](http://www.jianshu.com/p/b3a40a55826e)

***