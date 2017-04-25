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
	```xml
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
	```java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_my);
		Toolbar myToolbar = (Toolbar) findViewById(R.id.my_toolbar);
		setSupportActionBar(myToolbar);
    }
	```
	
5. 这样便能显示最基础的 toolbar 。（带有 app name，背景颜色为 colorPrimary ）

### 二、为 toolbar 设置一个搜索按钮，和一个设置按钮（该按钮是隐藏的）
1. 在 res/menu 下新建一个XML文件取名toolbar_main：
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<menu xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:app="http://schemas.android.com/apk/res-auto">
		<!-- "search", should appear as action button if possible -->
		<item
			android:id="@+id/action_search"
			android:icon="@drawable/ic_action_search"
			android:title="@string/action_search"
			app:showAsAction="ifRoom"
			app:actionViewClass="android.support.v7.widget.SearchView"/>

		<!-- Settings, should always be in the overflow -->
		<item android:id="@+id/action_settings"
			android:title="@string/action_settings"
			app:showAsAction="never"/>

	</menu>
	```
	
2. 在需要 Toolbar 的活动中，先 findViewById 找到 toolbar 控件,然后 setSupportActionBar ，然后如下：
	```java
	@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.toolbar_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.action_settings:
                return true;

            case R.id.action_search:
                return true;

            default:
                return super.onOptionsItemSelected(item);

        }
    }
	```

3. 效果就是：左边为app的名字，右边依次是一个搜索的图标和一个三竖点的图标。点击搜索按钮，出现下划线供输入；点击三点，出现设置按钮。


### 三、为 toolbar 增加一个返回按钮
1. 首先要有一个 parentActivity （这样按了返回按钮才有去处）。   

2. 在 AndroidManifest.xml 中声明子活动的父活动是谁，如下：   
	![mf.PNG](https://ooo.0o0.ooo/2017/03/19/58ce3ea9970e0.png)     
	
3. 在子活动的 onCreate 方法里设置显示返回按钮：   
	```java
	    Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        // 获得ActionBar的对象
        ActionBar ab = getSupportActionBar();
        // 增加一个up按钮
        ab.setDisplayHomeAsUpEnabled(true);
	```

4. 如此就实现了返回按钮功能啦。   

5. 但是，当我们按一下返回按钮回到之前的 Activity 后，那个 Activity 会重新创建，即重新执行 onCreate 方法。这不是我们所希望的，我们所想要的是和 back 键一样的功能。所以，我们重新实现一下 up 键的功能。   
	整理一下，我们要有一个父活动和一个子活动 ，点击子活动中 toolbar 上的 up 按钮，返回到父活动。    
	在子活动的布局界面中添加一个 Toolbar ，和上面代码一样。     
	在子活动中：
	```
	@Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Toolbar toolbar = (Toolbar) findViewById(R.id.tb_up);

        if (toolbar != null) {
            setSupportActionBar(toolbar);
            // 获得ActionBar的对象
            ActionBar ab = getSupportActionBar();
            // 增加一个up按钮
            ab.setDisplayHomeAsUpEnabled(true);
            // 隐藏标题
            ab.setDisplayShowTitleEnabled(false);
            ToolbarHelper toolbarHelper = new ToolbarHelper(toolbar);
            handleToolbar(toolbarHelper);
        }
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // 点击了toolbar中的up向上按钮
        if(item.getItemId()==android.R.id.home){
            finish();//结束当前，返回上一级
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
	```
	这样就可以啦。不需要在清单文件中声明父活动是谁。


### 四、自定义 toolbar （左边一个返回按钮，中间显示标题）

1. 在 layout 中增加一个 toolbar.xml ，增加一个 TextView 。内容如下：  
	![toolbar_xml.PNG](https://ooo.0o0.ooo/2017/03/19/58ce4933cf740.png)   

2. 增加一个布局文件 activity_classification.xml ，引入 toolbar 的布局：   
	![include.PNG](https://ooo.0o0.ooo/2017/03/19/58ce495d2cfb9.png)   

3. 创建一个 BaseToolbarActivity 作为父类,设置不显示默认标题，显示返回按钮;创建一个 toolbar 的帮助类：  
	```java
	package team.xinfe.shiyu.ui;

	import android.os.Bundle;
	import android.support.annotation.Nullable;
	import android.support.v7.app.ActionBar;
	import android.support.v7.app.AppCompatActivity;
	import android.support.v7.widget.SearchView;
	import android.support.v7.widget.Toolbar;
	import android.util.Log;
	import android.view.MenuItem;
	import android.view.View;
	import android.widget.TextView;

	import team.xinfe.shiyu.R;

	/**
	 * 含up按钮的toolbar
	 * Created by xinfe on 2017/3/19.
	 */

	public class ToolBarUpActivity extends AppCompatActivity {

		@Override
		protected void onCreate(@Nullable Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			Toolbar toolbar = (Toolbar) findViewById(R.id.tb_up);

			if (toolbar != null) {
				setSupportActionBar(toolbar);
				// 获得ActionBar的对象
				ActionBar ab = getSupportActionBar();
				// 增加一个up按钮
				ab.setDisplayHomeAsUpEnabled(true);
				// 隐藏标题
				ab.setDisplayShowTitleEnabled(false);
				ToolbarHelper toolbarHelper = new ToolbarHelper(toolbar);
				handleToolbar(toolbarHelper);
			}
		}

		@Override
		public boolean onOptionsItemSelected(MenuItem item) {
			// 点击了toolbar中的up向上按钮
			if(item.getItemId()==android.R.id.home){
				finish();//结束当前，返回上一级
				return true;
			}
			return super.onOptionsItemSelected(item);
		}


		/**
		 * 由继承该活动的子活动完成处理功能
		 * @param toolbarHelper ToolbarHelper
		 */
		protected void handleToolbar(ToolbarHelper toolbarHelper) {}


		/**
		 * toolbar帮助类
		 */
		public static class ToolbarHelper {
			private Toolbar mToolbar;

			public ToolbarHelper(Toolbar toolbar) {
				this.mToolbar = toolbar;
			}

			public Toolbar getToolbar() {
				return mToolbar;
			}

			//为toolbar设置标题
			public void setTitle(String title) {
				TextView tvTitle = (TextView) mToolbar.findViewById(R.id.tv_toolbar_title);
				tvTitle.setText(title);
			}

			//为toolbar设置标题
			public void setTitle(int title) {
				TextView tvTitle = (TextView) mToolbar.findViewById(R.id.tv_toolbar_title);
				tvTitle.setText(title);
			}
		}
	}

	```
  

4. 创建一个继承 BaseToolbarActivity 的子类 ClassificationActivity ，重写方法：      
	```java
	package team.xinfe.shiyu.ui;

	import android.os.Bundle;
	import android.util.Log;

	import team.xinfe.shiyu.R;

	/**
	 * 分类
	 * Created by xinfe on 2017/3/19.
	 */

	public class ClassificationActivity extends BaseToolBarActivity {
		private static final String TAG = "ClassificationActivity";

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			//先设置ContentView，再调用父类onCreate，不然父类找不到toolbar的id
			setContentView(R.layout.activity_classification);
			super.onCreate(savedInstanceState);
			Log.d("shiyu",TAG+"onCreate");

		}

		//重写父类的handleToolbar方法
		protected void handleToolbar(ToolbarHelper toolbarHelper) {
			Log.d("shiyu",TAG+"handleToolbar");
			toolbarHelper.setTitle("分类");
		}
	}
	```


6. 效果：   
	![home.png](https://ooo.0o0.ooo/2017/03/19/58ce4e0fe52a5.png)   
	![ClassificationActivity.png](https://ooo.0o0.ooo/2017/03/19/58ce4e2c73dda.png)   
	
	

### 参考资料
- [Android Developers](https://developer.android.com/training/appbar/setting-up.html)   
- [Toolbar使用详解](http://www.jianshu.com/p/b3a40a55826e)

***
