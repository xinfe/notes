*author : xinfe*   
*date : 2017-03-18*
***
# 关于Android(1)BottomNavigationBar和Fragment的综合使用

### 一、下载
直接在gradle中抓取：
![grab.PNG](https://ooo.0o0.ooo/2017/03/18/58cc99ec7dbba.png)  


### 二、使用([Usage](https://github.com/Ashok-Varma/BottomNavigation/wiki/Usage))

1. 在布局文件中写入：      
	![xml中的使用.PNG](https://ooo.0o0.ooo/2017/03/18/58ccba779c1c4.png)   

2. 在Activity中写入：      
	```java
	    BottomNavigationBar bottomNavigationBar = (BottomNavigationBar) findViewById(R.id.bottom_navigation_bar);

        bottomNavigationBar
                .setMode(BottomNavigationBar.MODE_FIXED)    // Values:MODE_DEFAULT, MODE_FIXED, MODE_SHIFTING(移位)
                //Values: BACKGROUND_STYLE_DEFAULT, BACKGROUND_STYLE_STATIC, BACKGROUND_STYLE_RIPPLE(波纹)
                .setBackgroundStyle(BottomNavigationBar.BACKGROUND_STYLE_STATIC)
                .setActiveColor(R.color.colorPrimary)       //选中时tab的颜色
                .setInActiveColor(R.color.Black)            //未选中时tab的颜色
                .setBarBackgroundColor(R.color.White)       //背景颜色
                .addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "首页"))
                .addItem(new BottomNavigationItem(R.drawable.ic_add_white_24dp, "添加"))
                .addItem(new BottomNavigationItem(R.drawable.ic_me_white_24dp, "我的"))
                .initialise();
        bottomNavigationBar.setTabSelectedListener(this);
	```

	
 
3. 也可为每个tab都设置“选中”“未选中”的颜色：    
	```java
	bottomNavigationBar
		.addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setActiveColor(R.color.orange).setInActiveColor(R.color.teal))
		.addItem(new BottomNavigationItem(R.drawable.ic_book_white_24dp, "Books").setActiveColor("#FFFF00"))
		.addItem(new BottomNavigationItem(R.drawable.ic_music_note_white_24dp, "Music").setInActiveColor("#00FFFF"))
		.addItem(new BottomNavigationItem(R.drawable.ic_tv_white_24dp, "Movies & TV"))
		.addItem(new BottomNavigationItem(R.drawable.ic_videogame_asset_white_24dp, "Games").setActiveColor(R.color.grey))
	```

4. 也可设置不同的图片(即未点击时是一种图片，点击之后是另一种图片)     
	```java
	new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setInactiveIcon(R.drawable.ic_home_black)
	```



### 三、实例
1. activity_main.xml  
	![layout.PNG](https://ooo.0o0.ooo/2017/03/18/58ccbad8ca519.png)

2. MainActivity.java  
	```java
	package team.xinfe.shiyu;
	
	import android.app.Fragment;
	import android.app.FragmentManager;
	import android.app.FragmentTransaction;
	import android.os.Bundle;
	import android.support.v7.app.AppCompatActivity;
	import android.util.Log;
	import android.view.KeyEvent;
	import android.widget.Toast;
	
	import com.ashokvarma.bottomnavigation.BottomNavigationBar;
	import com.ashokvarma.bottomnavigation.BottomNavigationItem;
	
	import team.xinfe.shiyu.fragments.AddFragment;
	import team.xinfe.shiyu.fragments.HomeFragment;
	import team.xinfe.shiyu.fragments.MeFragment;
	/**
	* 主界面（Fragment，BottomNavigationBar）
	* Created by xinfe on 2017/3/5.
	*/
	public class MainActivity extends AppCompatActivity implements BottomNavigationBar.OnTabSelectedListener{
		private static final String TAG="MainActivity";
		private Fragment homeFragment,addFragment,meFragment;
		private long exitTime = 0;
		private int index = 0;
	
		protected void onCreate(Bundle savedInstanceState) {
			Log.d("shiyu", MainActivity.TAG+"onCreate");
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
	
			BottomNavigationBar bottomNavigationBar = (BottomNavigationBar) findViewById(R.id.bottom_navigation_bar);
	
			bottomNavigationBar
					.setMode(BottomNavigationBar.MODE_FIXED)    // Values:MODE_DEFAULT, MODE_FIXED, MODE_SHIFTING(移位)
					//Values: BACKGROUND_STYLE_DEFAULT, BACKGROUND_STYLE_STATIC, BACKGROUND_STYLE_RIPPLE(波纹)
					.setBackgroundStyle(BottomNavigationBar.BACKGROUND_STYLE_STATIC)
					.setActiveColor(R.color.colorPrimary)       //选中时tab的颜色
					.setInActiveColor(R.color.Black)            //未选中时tab的颜色
					.setBarBackgroundColor(R.color.White)       //背景颜色
					.addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "首页"))
					.addItem(new BottomNavigationItem(R.drawable.ic_add_white_24dp, "添加"))
					.addItem(new BottomNavigationItem(R.drawable.ic_me_white_24dp, "我的"))
					.initialise();
			bottomNavigationBar.setTabSelectedListener(this);
			setDefaultFragment();
		}
	
		@Override
		public void onTabSelected(int position) {
			Log.d("shiyu",TAG+"onTabSelected");
			switch (position) {
				case 0:
					if (homeFragment == null) {
						homeFragment = new HomeFragment();
					}
					if(index == 1){
						switchFragment(addFragment,homeFragment);
					}
					if(index == 2){
						switchFragment(meFragment,homeFragment);
					}
					index = 0;
					break;
				case 1:
					if (addFragment == null) {
						addFragment = new AddFragment();
					}
					if(index == 0){
						switchFragment(homeFragment,addFragment);
					}
					if(index == 2){
						switchFragment(meFragment,addFragment);
					}
					index = 1;
					break;
				case 2:
					if (meFragment == null) {
						meFragment = new MeFragment();
					}
					if(index == 0){
						switchFragment(homeFragment,meFragment);
					}
					if(index == 1){
						switchFragment(addFragment,meFragment);
					}
					index = 2;
					break;
				default:
					break;
			}
		}

		@Override
		public void onTabUnselected(int position) {
			Log.d("shiyu",TAG+"onTabUnselected");
		}

		@Override
		public void onTabReselected(int position) {
			Log.d("shiyu",TAG+"onTabReselected");
		}

		//设置默认显示的fragment
		private void setDefaultFragment(){
			Log.d("shiyu", MainActivity.TAG+"setDefaultFragment");
			FragmentManager fm = getFragmentManager();
			FragmentTransaction transaction = fm.beginTransaction();
			homeFragment = new HomeFragment();
			transaction.add(R.id.fl_mainContent, homeFragment).show(homeFragment);
			transaction.commit();
		}

		/**
		 * 从 Fragment from 切换到 Fragment to
		 *首先判断 to 是否已经 add 过
		 *如果没有 add，则 hide from，add to
		 *已经 add 过，表明已经存在 to 实例，
		 * 只需 hide 当前显示的 from，show to
		 */
		public void switchFragment(Fragment from, Fragment to) {
			FragmentManager fm = getFragmentManager();
			FragmentTransaction transaction = fm.beginTransaction();
			if (!to.isAdded()) { // 先判断是否被add过
				transaction.hide(from)
						.add(R.id.fl_mainContent, to)
						.commit();
			} else {
				transaction.hide(from).show(to).commit();
			}
		}

		//再按一次退出程序
		public boolean onKeyDown(int keyCode,KeyEvent event){

			if(keyCode == KeyEvent.KEYCODE_BACK && event.getAction() == KeyEvent.ACTION_DOWN){
				if((System.currentTimeMillis()-exitTime) > 2000){
					Toast.makeText(getApplicationContext(), "再按一次退出程序", Toast.LENGTH_SHORT).show();
					exitTime = System.currentTimeMillis();
				} else {
					finish();
					System.exit(0);
				}
				return true;
			}
			return super.onKeyDown(keyCode, event);
		}

		/**
		 * 阻止activity回滚数据
		 * 当APP到后台之后，activity和fragment会执行onSaveInstanceState去保存数据
		 * 而当内存不足或者放置了很久的时间，activity的视图view以及里面装的fragment被销毁了，
		 * 而fragment实例还存在，
		 * 为防止fragment重叠，故注释掉回滚
		 */
		@Override
		protected void onRestoreInstanceState(Bundle savedInstanceState) {

			//super.onRestoreInstanceState(savedInstanceState);
		}
	}
	```
	

	
3. 效果   
	![bottom.png](https://ooo.0o0.ooo/2017/03/19/58ce5d03841bd.png)   


### 四、注意   
1. v4包下支持的Fragment和app包下的Fragment   

	**v4包的FragmentManager:**
	```java
	//3.0以下版本没有fragment的api,必须借助V4包里面的getSupportFragmentManager()方法来获取FragmentManager()对象。
	//而且getSupportFragmentManager()有其运用范围，只能在部分activity中运用。
	//当遇到getSupportFragmentManager()没定义的问题时，修改activity为FragmentActivity或者AppCompatActivity。
	
	FragmentManager fragmentManager = getSupportFragmentManager();
	```
	**app包下FragmentManager:**
	```java
	//3.0版本之后，有了Fragment的api，就可以直接使用getFragmentManager()这个方法来获取了
	
	Fragmentmanager fragmentManager = getFragmentManager();
	```

2. Fragment的replace()和hide(),add(),show()   

	在项目中切换Fragment，一直都是用replace()方法来替换Fragment。
	但是这样做有一个问题，每次切换的时候Fragment都会重新实列化，重新加载一次数据，这样做会非常消耗性能和用户的流量。   
	
	官方文档解释说：replace()这个方法只是在上一个Fragment不再需要时采用的简便方法。   
	较好的切换方法是使用hide,add,show方法。   
	
	思路是：点击一个按钮，要显示某个fragment时，
	首先判断该fragment是否已经add过，
	如果没有add，则hide当前fragment，add该fragment，
	已经add过，表明已经存在该fragment实例，只需hide当前显示的fragment，show此fragment。
	这样就能做到多个Fragment切换不重新实例化，但当内存不足时也会造成界面重叠问题。   
	

3. 内存不足Fragment界面重叠的解决办法：
	- 状态检测：[Android Frament的切换（解决replace的低效）](http://www.cnblogs.com/android-joker/p/4414891.html) [ fragment界面重叠问题的终极解决方法 ](http://blog.csdn.net/fussenyu/article/details/51283282)
	- 阻止数据回滚：[ fragment重叠问题（add hide show方式）](http://blog.csdn.net/fly7632785/article/details/48295741)



### 参考资料
- [Github - BottomNavigation](https://github.com/Ashok-Varma/BottomNavigation)   
- [Bottom navigation - Components - Material design guidelines](https://material.io/guidelines/components/bottom-navigation.html#bottom-navigation-specs)
***
