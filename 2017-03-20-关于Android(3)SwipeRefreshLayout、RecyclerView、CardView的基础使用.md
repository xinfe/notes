*author : xinfe*   
*date : 2017-03-20*
***
# 关于Android(3)SwipeRefreshLayout、RecyclerView、CardView的基础使用


### 实例：
1. gradle中增加以下代码：
	```java
	compile 'com.android.support:cardview-v7:21.0.+'
    compile 'com.android.support:recyclerview-v7:21.0.+'
	```
	
2. 布局：fragment_home.xml(最上方是自制的一个标题栏，然后是 SwipeRefreshLayout 控件，控件中还有一个RecyclerView)   
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:orientation="vertical">

		<LinearLayout
			android:layout_width="match_parent"
			android:layout_height="64dp"
			android:orientation="horizontal"
			android:background="@color/colorPrimary">
			<!--自制的标题栏，代码就不贴了-->

		</LinearLayout>

		<android.support.v4.widget.SwipeRefreshLayout
			xmlns:android="http://schemas.android.com/apk/res/android"
			android:id="@+id/swipeRefresh"
			android:layout_width="match_parent"
			android:layout_height="match_parent">
			
			<android.support.v7.widget.RecyclerView
				android:id="@+id/my_recycler_view"
				android:scrollbars="vertical"
				android:layout_width="match_parent"
				android:layout_height="match_parent"/>

		</android.support.v4.widget.SwipeRefreshLayout>
	</LinearLayout>
	```

2. 布局：item.xml( RecyclerView 里的子项布局,表现为 RecyclerView 里的每一个 CardView 里有一张图，和中央一个标题)
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:orientation="vertical" android:layout_width="match_parent"
		android:padding="10dp"
		android:layout_height="wrap_content">

		<android.support.v7.widget.CardView
			xmlns:card_view="http://schemas.android.com/apk/res-auto"
			android:id="@+id/card_view"
			android:layout_width="match_parent"
			android:layout_height="200dp"
			card_view:cardCornerRadius="4dp">

			<RelativeLayout
				android:layout_width="match_parent"
				android:layout_height="match_parent">
				<ImageView
					android:id="@+id/bg"
					android:layout_width="match_parent"
					android:layout_height="match_parent" />
				<TextView
					android:id="@+id/title"
					android:layout_width="wrap_content"
					android:layout_height="wrap_content"
					android:textSize="24sp"
					android:textColor="@color/Black"
					android:layout_gravity="center"
					android:layout_centerInParent="true"/>
			</RelativeLayout>

		</android.support.v7.widget.CardView>

	</LinearLayout>
	```
	
3. 数据：Bean.java
	```java
	package team.xinfe.shiyu.model;

	/**
	 * 测试用例
	 * Created by xinfe on 2017/3/20.
	 */

	public class Bean {
		private String mTitle;
		private String mContent;
		private int mImageId;

		public Bean(String title,String content,int imageId){
			mTitle = title;
			mContent = content;
			mImageId = imageId;
		}

		public String getTitle(){
			return mTitle;
		}

		public void setTitle(String title){
			mTitle = title;
		}

		public String getContent(){
			return mContent;
		}

		public void setContent(String content){
			mContent = content;
		}

		public int getImageId(){
			return mImageId;
		}

		public void setImageId(int imageId){
			mImageId = imageId;
		}
	}
	```
	
4. 适配器：RecyclerViewAdapter.java(用于将数据显示到每一个item布局上)
	```java
	package team.xinfe.shiyu.adapters;

	import android.support.v7.widget.RecyclerView;
	import android.view.LayoutInflater;
	import android.view.View;
	import android.view.ViewGroup;
	import android.widget.ImageView;
	import android.widget.TextView;

	import java.util.List;

	import team.xinfe.shiyu.R;
	import team.xinfe.shiyu.model.Bean;

	/**
	 * Created by xinfe on 2017/3/19.
	 */

	public class RecyclerViewAdapter extends RecyclerView.Adapter<RecyclerViewAdapter.ViewHolder> {
		private List<Bean> mDataset;

		// 获得数据
		public RecyclerViewAdapter(List<Bean> myDataset) {
			mDataset = myDataset;
		}

		// 创建ViewHolder的布局 (invoked by the layout manager)
		@Override
		public RecyclerViewAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {

			View v = LayoutInflater.from(parent.getContext())
					.inflate(R.layout.item, parent, false);
			//将布局传给ViewHolder，之后就可以调用finViewById找到相应的控件，然后就可以为相应控件设置内容了
			ViewHolder vh = new ViewHolder(v);
			return vh;
		}

		//通过ViewHolder将数据绑定到界面上进行显示 (invoked by the layout manager)
		@Override
		public void onBindViewHolder(RecyclerViewAdapter.ViewHolder holder, int position) {
			// 获取position处的数据
			// 并设置到item的布局中
			holder.mTextView.setText(mDataset.get(position).getTitle());
			holder.mImageView.setBackgroundResource(mDataset.get(position).getImageId());

		}

		//数据大小 (invoked by the layout manager)
		@Override
		public int getItemCount() {
			return mDataset.size();
		}


		public static class ViewHolder extends RecyclerView.ViewHolder {
			public ImageView mImageView;
			public TextView mTextView;
			public ViewHolder(View v) {
				super(v);
				mImageView = (ImageView) v.findViewById(R.id.bg);
				mTextView = (TextView)v.findViewById(R.id.title);
			}
		}

	}
	```

5. Fragment：HomeFragment.java(实现下拉刷新的监听来更新数据，实现填充 CardView 的数据的初始化)
	```java
	package team.xinfe.shiyu.fragments;

	import android.app.Fragment;
	import android.os.Bundle;
	import android.support.v4.widget.SwipeRefreshLayout;
	import android.support.v7.widget.LinearLayoutManager;
	import android.support.v7.widget.RecyclerView;
	import android.util.Log;
	import android.view.LayoutInflater;
	import android.view.View;
	import android.view.ViewGroup;

	import java.util.ArrayList;
	import java.util.List;

	import team.xinfe.shiyu.R;
	import team.xinfe.shiyu.adapters.RecyclerViewAdapter;
	import team.xinfe.shiyu.model.Bean;

	/**
	 * 首页（自制标题栏，RecyclerView）
	 * Created by xinfe on 2017/3/5.
	 */

	public class HomeFragment extends Fragment{
		private static final String TAG="HomeFragment";
		private SwipeRefreshLayout mSwipeRefreshLayout;
		private RecyclerView mRecyclerView;
		private RecyclerView.Adapter myRecyclerViewAdapter;
		private RecyclerView.LayoutManager mLayoutManager;
		private List<Bean> mData;

		@Override
		public void onCreate(Bundle savedInstanceState){
			super.onCreate(savedInstanceState);
			Log.d("shiyu",HomeFragment.TAG+"onCreate");
			initData();
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
			Log.d("shiyu",HomeFragment.TAG+"onCreateView");
			View view = inflater.inflate(R.layout.fragment_home,container,false);
			mSwipeRefreshLayout = (SwipeRefreshLayout)view.findViewById(R.id.swipeRefresh);
			mSwipeRefreshLayout.setOnRefreshListener(
					new SwipeRefreshLayout.OnRefreshListener() {
						@Override
						public void onRefresh() {//下拉刷新时会调用此方法
							//Log.i(LOG_TAG, "onRefresh called from SwipeRefreshLayout");

							// This method performs the actual data-refresh operation.
							// The method calls setRefreshing(false) when it's finished.
							updateOperation();
						}
					}
			);

			mRecyclerView = (RecyclerView) view.findViewById(R.id.my_recycler_view);

			// 线性布局管理器，支持横向和纵向形式
			mLayoutManager = new LinearLayoutManager(this.getActivity());
			mRecyclerView.setLayoutManager(mLayoutManager);

			// 设置adapter
			myRecyclerViewAdapter = new RecyclerViewAdapter(mData);
			mRecyclerView.setAdapter(myRecyclerViewAdapter);
			myRecyclerViewAdapter.notifyDataSetChanged();

			return view;
		}


		// 初始化数据
		private void initData(){
			mData = new ArrayList<>();
			mData.add(new Bean("初始测试用例","content",R.drawable.bg));
		}

		//更新操作
		public void updateOperation(){
			mData.add(new Bean("测试用例新增","content",R.drawable.bg));
			mSwipeRefreshLayout.setRefreshing(false);
		}
	}
	```

5. 效果：(由于更新速度较快，进度条没截到)   
	![初始化.png](https://ooo.0o0.ooo/2017/03/20/58cf8dd877f66.png) 
		
	![更新.png](https://ooo.0o0.ooo/2017/03/20/58cf8dd98035d.png)



### 参考资料
- [Android Developer - Adding Swipe-to-Refresh To Your App](https://developer.android.com/training/swipe/add-swipe-interface.html#AddRefreshAction)   
- [Android Developer - 创建列表与卡片](https://developer.android.com/training/material/lists-cards.html)   
- [Android Developer - API](https://developer.android.com/reference/packages.html)   
- [CSDN - Android新特性之RecyclerView的简单使用 ](http://blog.csdn.net/mr_dsw/article/details/48809429)

***