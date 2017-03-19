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
	![activity中的使用.PNG](https://ooo.0o0.ooo/2017/03/18/58ccbaaa1fb62.png)   
 
3. 也可为每个tab都设置“选中”“未选中”的颜色：    
	```
	bottomNavigationBar
		.addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setActiveColor(R.color.orange).setInActiveColor(R.color.teal))
		.addItem(new BottomNavigationItem(R.drawable.ic_book_white_24dp, "Books").setActiveColor("#FFFF00"))
		.addItem(new BottomNavigationItem(R.drawable.ic_music_note_white_24dp, "Music").setInActiveColor("#00FFFF"))
		.addItem(new BottomNavigationItem(R.drawable.ic_tv_white_24dp, "Movies & TV"))
		.addItem(new BottomNavigationItem(R.drawable.ic_videogame_asset_white_24dp, "Games").setActiveColor(R.color.grey))
	```

4. 也可设置不同的图片(即未点击时是一种图片，点击之后是另一种图片)     
	```
	new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setInactiveIcon(R.drawable.ic_home_black)
	```



### 三、实例
1. activity_main.xml  
	![layout.PNG](https://ooo.0o0.ooo/2017/03/18/58ccbad8ca519.png)

2. MainActivity.java  
	![Main.PNG](https://ooo.0o0.ooo/2017/03/19/58ce59370af9f.png)   
	![onCreate.PNG](https://ooo.0o0.ooo/2017/03/19/58ce5937159ce.png)   
	![onTabSelected.png](https://ooo.0o0.ooo/2017/03/19/58ce59371132a.png)   
	![defaultFragment.PNG](https://ooo.0o0.ooo/2017/03/19/58ce5936aa487.png)   
	![switchFragment.PNG](https://ooo.0o0.ooo/2017/03/19/58ce59366bcb4.png)   
	![keydown.PNG](https://ooo.0o0.ooo/2017/03/19/58ce593701a2d.png)   

	
3. 效果   
	![bottom.png](https://ooo.0o0.ooo/2017/03/19/58ce5d03841bd.png)   


### 四、注意   
1. v4包下支持的Fragment和app包下的Fragment   

	**v4包的FragmentManager:**
	```
	//3.0以下版本没有fragment的api,必须借助V4包里面的getSupportFragmentManager()方法来获取FragmentManager()对象。
	//而且getSupportFragmentManager()有其运用范围，只能在部分activity中运用。
	//当遇到getSupportFragmentManager()没定义的问题时，修改activity为FragmentActivity或者AppCompatActivity。
	
	FragmentManager fragmentManager = getSupportFragmentManager();
	```
	**app包下FragmentManager:**
	``` 
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
	
	![switchFragment.PNG](https://ooo.0o0.ooo/2017/03/19/58ce36dee0ffa.png)

3. 内存不足Fragment界面重叠的解决办法：
	- 状态检测：[Android Frament的切换（解决replace的低效）](http://www.cnblogs.com/android-joker/p/4414891.html) [ fragment界面重叠问题的终极解决方法 ](http://blog.csdn.net/fussenyu/article/details/51283282)
	- 阻止数据回滚：[ fragment重叠问题（add hide show方式）](http://blog.csdn.net/fly7632785/article/details/48295741)



### 参考资料
[Github - BottomNavigation](https://github.com/Ashok-Varma/BottomNavigation)   
[Bottom navigation - Components - Material design guidelines](https://material.io/guidelines/components/bottom-navigation.html#bottom-navigation-specs)
***
