*author : xinfe*   
*date : 2017-04-06*
***
# 关于Android(6)GalleryFinal和Glide的基础使用

### 效果：   
**点击图中加号**   
![点击加号.png](https://ooo.0o0.ooo/2017/04/06/58e647b9f3c5d.png)  
**选择图片（最多选择九张）**   
![选择图片.png](https://ooo.0o0.ooo/2017/04/06/58e65991bfad1.png)   
**显示在指定地点**   
![显示图片.png](https://ooo.0o0.ooo/2017/04/06/58e6598f3ee4a.png)   


### 使用的技术：
[GalleryFinal](https://github.com/pengjianbo/GalleryFinal)拥有自定义相册、图片选择、图片预览、裁剪旋转等等功能。   
[Glide](https://github.com/bumptech/glide)是一种ImageLoader。      


### 一、修改gradle   
1. 抓取
	```java
	compile 'com.github.bumptech.glide:glide:3.7.0'
    compile 'cn.finalteam:galleryfinal:1.4.8.7'
    compile 'com.android.support:support-v4:23.1.1'
	```
	
2. 修改targetSdkVersion为23。   


### 二、xml布局
1. activity_add_question.xml(包含一个Toolbar，Toolbar在此不做讲述，ScrollView中包含两个TextView,一个ImageView，一个GridView)
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:orientation="vertical" android:layout_width="match_parent"
		android:layout_height="match_parent">

		<include layout="@layout/toolbar_publish"/>

		<ScrollView
			android:layout_width="match_parent"
			android:layout_height="wrap_content">
			<LinearLayout
				android:layout_width="match_parent"
				android:layout_height="wrap_content"
				android:orientation="vertical"
				android:weightSum="1">
				<EditText
					android:id="@+id/et_add_question_title"
					android:layout_width="match_parent"
					android:layout_height="wrap_content"
					android:hint="一句话概括你的问题"
					android:gravity="center"
					android:background="@null"
					android:textSize="@dimen/title_text_size"
					android:padding="@dimen/activity_vertical_margin"/>

				<EditText
					android:id="@+id/et_add_question_content"
					android:layout_width="match_parent"
					android:layout_height="wrap_content"
					android:hint="具体描述"
					android:background="@null"
					android:textSize="@dimen/primary_text_size"
					android:padding="@dimen/activity_vertical_margin"/>

				<ImageView
					android:id="@+id/iv_add_question_image"
					android:layout_width="96dp"
					android:layout_height="96dp"
					android:layout_marginLeft="@dimen/activity_horizontal_margin"
					android:layout_marginStart="@dimen/activity_horizontal_margin"
					android:layout_marginTop="@dimen/activity_vertical_margin"
					android:src="@drawable/ic_action_add_picture"/>

				<GridView
					android:id="@+id/gv_photo_list"
					android:layout_width="match_parent"
					android:layout_height="290dp"
					android:numColumns="auto_fit"
					android:verticalSpacing="10dp"
					android:paddingLeft="@dimen/activity_horizontal_margin"
					android:paddingStart="@dimen/activity_horizontal_margin"
					android:columnWidth="96dp"
					android:stretchMode="columnWidth">
				</GridView>

			</LinearLayout>

		</ScrollView>


	</LinearLayout>
	```
	
2. item_add_photo.xml(GridView的子项布局，就是一个ImageView)   
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<ImageView xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="96dp"
		android:layout_height="96dp">

	</ImageView>
	```
	
### 三、配置
1. 新建一个class 取名 MyApplication 继承 Application   
	```java
	package team.xinfe.shiyu;

	import android.app.Application;
	import android.os.Environment;

	import java.io.File;

	import cn.finalteam.galleryfinal.CoreConfig;
	import cn.finalteam.galleryfinal.FunctionConfig;
	import cn.finalteam.galleryfinal.GalleryFinal;
	import cn.finalteam.galleryfinal.ImageLoader;
	import cn.finalteam.galleryfinal.ThemeConfig;
	import team.xinfe.shiyu.listener.GlidePauseOnScrollListener;
	import team.xinfe.shiyu.loader.GlideImageLoader;

	/**
	 * MyApplication
	 * 当android程序启动时系统会创建一个 application对象，用来存储系统的一些信息。
	 * 通常我们是不需要指定一个Application的，系统会为每个程序运行时创建一个Application类的对象且仅创建一个，
	 * 所以Application可以说是单例 (singleton)模式的一个类.
	 * 可以通过Application来进行一些，数据传递，数据共享,数据缓存等操作。
	 * Created by xinfe on 2017/4/6.
	 */

	public class MyApplication extends Application {


		@Override
		public void onCreate() {
			super.onCreate();

			File takePhotoFolder = new File(Environment.getExternalStorageDirectory()+"/Shiyu/take/");
			File editPhotoFolder = new File(Environment.getExternalStorageDirectory()+"/Shiyu/edit/");

			//默认主题为DEFAULT（深蓝色）,还自带主题：DARK（黑色）、CYAN（蓝绿）、ORANGE（橙色）、GREEN（绿色）和TEAL（青绿色），
			//当然也支持自定义主题（Custom Theme）,在自定义主题中用户可以配置字体颜色、图标颜色、更换图标、和背景色
			ThemeConfig theme = ThemeConfig.GREEN;

			//配置功能
			FunctionConfig functionConfig = new FunctionConfig.Builder()
					.setEnableCamera(true)//开启相机功能
					.setEnableEdit(true)//开启编辑功能
					.setEnableCrop(true)//开启裁剪功能
					.setCropSquare(true)//裁剪正方形
					//.setMutiSelectMaxSize(9)//多选最多为9张
					.setEnablePreview(true)//开启预览功能
					.build();

			//配置imageLoader
			ImageLoader imageLoader = new GlideImageLoader();

			CoreConfig coreConfig = new CoreConfig.Builder(this, imageLoader, theme)
					.setNoAnimcation(true)//关闭动画
					.setFunctionConfig(functionConfig)
					.setTakePhotoFolder(takePhotoFolder)//设置拍照保存目录，默认是/sdcard/DICM/GalleryFinal/
					.setEditPhotoCacheFolder(editPhotoFolder)//配置编辑（裁剪和旋转）功能产生的cache文件保存目录，不做配置的话默认保存在/sdcard/GalleryFinal/edittemp/
					.setPauseOnScrollListener(new GlidePauseOnScrollListener(false,true))//设置imageLoader滑动加载图片优化OnScrollListener,根据选择的ImageLoader来选择PauseOnScrollListener
					.build();
			GalleryFinal.init(coreConfig);
		}
	}
	```
	
2. GlideImageLoader.java(github上都有，我用的是Glide)
	```java
	package team.xinfe.shiyu.loader;

	import android.app.Activity;
	import android.graphics.drawable.Drawable;

	import com.bumptech.glide.Glide;
	import com.bumptech.glide.load.engine.DiskCacheStrategy;
	import com.bumptech.glide.load.resource.drawable.GlideDrawable;
	import com.bumptech.glide.request.Request;
	import com.bumptech.glide.request.target.ImageViewTarget;

	import cn.finalteam.galleryfinal.ImageLoader;
	import cn.finalteam.galleryfinal.widget.GFImageView;

	/**
	 * GlideImageLoader
	 * Created by xinfe on 2017/4/6.
	 */

	public class GlideImageLoader implements ImageLoader {
		@Override
		public void displayImage(Activity activity, String path,final GFImageView imageView, Drawable defaultDrawable, int width, int height) {
			Glide.with(activity)
					.load("file://" + path)
					.placeholder(defaultDrawable)
					.error(defaultDrawable)
					.override(width, height)
					.diskCacheStrategy(DiskCacheStrategy.NONE) //不缓存到SD卡
					.skipMemoryCache(true)
					//.centerCrop()
					.into(new ImageViewTarget<GlideDrawable>(imageView) {
						@Override
						protected void setResource(GlideDrawable resource) {
							imageView.setImageDrawable(resource);
						}

						@Override
						public void setRequest(Request request) {
							//imageView.setTag(R.id.adapter_item_tag_key,request);
							imageView.setTag(request);//?
						}

						@Override
						public Request getRequest() {
							//return (Request) imageView.getTag(R.id.adapter_item_tag_key);
							return (Request) imageView.getTag();//?
						}
					});

		}

		@Override
		public void clearMemoryCache() {

		}
	}
	```

3. 将自定义的MyApplication设置到AndroidMenifest.xml中,即修改标签application的name，同时设置权限   
	```java
	<!-- Google Play will allow devices without a camera to download your application. -->
    <uses-feature android:name="android.hardware.camera"
        android:required="false" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        android:name=".MyApplication">
		...
        <activity android:name=".ui.ToolBarPublishActivity"/>
        <activity android:name=".ui.AddQuestionActivity"/>
		
    </application>
	```
	
### 四、ChoosePhotoListAdapter.java   
GridView的适配器，在构造函数中注意将activity传进来，以便Glide使用
```java
package team.xinfe.shiyu.adapters;

import android.app.Activity;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ImageView;

import com.bumptech.glide.Glide;

import java.util.List;

import cn.finalteam.galleryfinal.model.PhotoInfo;
import team.xinfe.shiyu.R;

/**
 * ChoosePhotoListAdapter
 * Created by xinfe on 2017/4/6.
 */

public class ChoosePhotoListAdapter extends BaseAdapter {
	private List<PhotoInfo> mPhotoList;
	private Activity mActivity;

	public ChoosePhotoListAdapter(Activity activity,List<PhotoInfo> photoList){
		mPhotoList = photoList;
		mActivity = activity;
	}


	@Override
	public int getCount() {
		return mPhotoList.size();
	}

	@Override
	public Object getItem(int position) {
		return position;
	}

	@Override
	public long getItemId(int position) {
		return position;
	}

	@Override
	public View getView(int position, View convertView, ViewGroup parent) {
		ImageView imageView = (ImageView)LayoutInflater.from(mActivity).inflate(R.layout.item_add_photo,parent,false);//加载Item布局
		PhotoInfo photoInfo = mPhotoList.get(position);//获取position处的PhotoInfo对象
		Glide.with(mActivity).load(photoInfo.getPhotoPath()).into(imageView);//加载PhotoInfo对象的路径，显示到ItemView上
		return imageView;
	}
}
```
	

### 五、AddQuestionActivity.java    
```java
package team.xinfe.shiyu.ui;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
import android.widget.GridView;
import android.widget.ImageView;
import android.widget.Toast;

import java.util.ArrayList;
import java.util.List;

import cn.finalteam.galleryfinal.GalleryFinal;
import cn.finalteam.galleryfinal.model.PhotoInfo;
import team.xinfe.shiyu.R;
import team.xinfe.shiyu.adapters.ChoosePhotoListAdapter;

/**
 * 提问
 * 继承ToolBarPublishActivity
 * Created by xinfe on 2017/3/30.
 */

public class AddQuestionActivity extends ToolBarPublishActivity{
    private static final String TAG = "AddQuestionActivity";
    static final int REQUEST_IMAGE_SELECT = 1;

    private EditText title;
    private EditText content;
    private ImageView imageView;
    private GridView gridView;
    private List<PhotoInfo> mPhotoList;
    private ChoosePhotoListAdapter mChoosePhotoListAdapter;

    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_add_question);
        super.onCreate(savedInstanceState);
        Log.d("shiyu",TAG+"onCreate");

        title = (EditText)findViewById(R.id.et_add_question_title);
        content = (EditText)findViewById(R.id.et_add_question_content);
        imageView = (ImageView) findViewById(R.id.iv_add_question_image);

        gridView = (GridView)findViewById(R.id.gv_photo_list);

        mPhotoList = new ArrayList<>();
        mChoosePhotoListAdapter = new ChoosePhotoListAdapter(this,mPhotoList);
        gridView.setAdapter(mChoosePhotoListAdapter);

        imageView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(mPhotoList.size() < 9){//判断此时是否已经有九张图，没有则可以打开相册继续选择
                    GalleryFinal.openGalleryMuti(REQUEST_IMAGE_SELECT, 9, mOnHandlerResultCallback);//开启相册，最多可选择9张照片
                }else{//已有九张则输出提示信息
                    Toast.makeText(AddQuestionActivity.this, "最多9张图哦", Toast.LENGTH_SHORT).show();
                }
            }
        });


//        imageView.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//                Intent selectPictureIntent = new Intent(Intent.ACTION_GET_CONTENT);
//                selectPictureIntent.setType("image/*");//获取内容是图片
//                /*
//                the startActivityForResult() method is protected by a condition that calls resolveActivity(),
//                which returns the first activity component that can handle the intent.
//                Performing this check is important because
//                if you call startActivityForResult() using an intent that no app can handle,
//                your app will crash. So as long as the result is not null, it's safe to use the intent.
//                 */
//                if (selectPictureIntent.resolveActivity(getPackageManager()) != null) {
//                    startActivityForResult(selectPictureIntent, REQUEST_IMAGE_SELECT);
//                }
//            }
//        });
    }

//    @Override
//    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
//        switch(requestCode){
//            case REQUEST_IMAGE_SELECT:
//                if(resultCode == RESULT_OK) {
//                    Uri imageUri = data.getData();
//                    Glide.with(this).load(imageUri).into(imageView);//将图片显示到ImageView中
//                }
//                break;
//            default:break;
//        }
//    }


    private GalleryFinal.OnHanlderResultCallback mOnHandlerResultCallback = new GalleryFinal.OnHanlderResultCallback() {
        @Override
        public void onHanlderSuccess(int requestCode, List<PhotoInfo> resultList) {
            if (resultList != null) {
                if(resultList.size()+mPhotoList.size() <= 9){//将图片总数限定在9张及以内
                    mPhotoList.addAll(resultList);//将选择的照片list存进mPhotoList中
                    mChoosePhotoListAdapter.notifyDataSetChanged();//监听数据变化
                }else{
                    Toast.makeText(AddQuestionActivity.this, "最多9张图哦", Toast.LENGTH_SHORT).show();
                }
            }
        }

        @Override
        public void onHanlderFailure(int requestCode, String errorMsg) {
            Toast.makeText(AddQuestionActivity.this, "error", Toast.LENGTH_SHORT).show();
        }
    };

}
```


### 参考资料
- [bumptech/glide: An image loading and caching library for Android focused on smooth scrolling](https://github.com/bumptech/glide)
- [Android图片加载框架最全解析（一），Glide的基本用法 - 郭霖的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/guolin_blog/article/details/53759439?utm_source=tuicool&utm_medium=referral)   
- [Android Application的作用 - lieren666的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/lieren666/article/details/7598288)   
- [Android四大图片缓存（Imageloader,Picasso,Glide,Fresco）原理、特性对比 - linghu_java - 博客园](http://www.cnblogs.com/linghu-java/p/5741358.html)   
- [Github - GalleryFinal](https://github.com/pengjianbo/GalleryFinal)     
***
