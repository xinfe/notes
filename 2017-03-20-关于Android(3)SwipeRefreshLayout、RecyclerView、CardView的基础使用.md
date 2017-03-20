*author : xinfe*   
*date : 2017-03-20*
***
# ����Android(3)SwipeRefreshLayout��RecyclerView��CardView�Ļ���ʹ��


### ʵ����
1. gradle���������´��룺
	```java
	compile 'com.android.support:cardview-v7:21.0.+'
    compile 'com.android.support:recyclerview-v7:21.0.+'
	```
	
2. ���֣�fragment_home.xml(���Ϸ������Ƶ�һ����������Ȼ���� SwipeRefreshLayout �ؼ����ؼ��л���һ��RecyclerView)   
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
			<!--���Ƶı�����������Ͳ�����-->

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

2. ���֣�item.xml( RecyclerView ��������,����Ϊ RecyclerView ���ÿһ�� CardView ����һ��ͼ��������һ������)
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
	
3. ���ݣ�Bean.java
	```java
	package team.xinfe.shiyu.model;

	/**
	 * ��������
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
	
4. ��������RecyclerViewAdapter.java(���ڽ�������ʾ��ÿһ��item������)
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

		// �������
		public RecyclerViewAdapter(List<Bean> myDataset) {
			mDataset = myDataset;
		}

		// ����ViewHolder�Ĳ��� (invoked by the layout manager)
		@Override
		public RecyclerViewAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {

			View v = LayoutInflater.from(parent.getContext())
					.inflate(R.layout.item, parent, false);
			//�����ִ���ViewHolder��֮��Ϳ��Ե���finViewById�ҵ���Ӧ�Ŀؼ���Ȼ��Ϳ���Ϊ��Ӧ�ؼ�����������
			ViewHolder vh = new ViewHolder(v);
			return vh;
		}

		//ͨ��ViewHolder�����ݰ󶨵������Ͻ�����ʾ (invoked by the layout manager)
		@Override
		public void onBindViewHolder(RecyclerViewAdapter.ViewHolder holder, int position) {
			// ��ȡposition��������
			// �����õ�item�Ĳ�����
			holder.mTextView.setText(mDataset.get(position).getTitle());
			holder.mImageView.setBackgroundResource(mDataset.get(position).getImageId());

		}

		//���ݴ�С (invoked by the layout manager)
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

5. Fragment��HomeFragment.java(ʵ������ˢ�µļ������������ݣ�ʵ����� CardView �����ݵĳ�ʼ��)
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
	 * ��ҳ�����Ʊ�������RecyclerView��
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
						public void onRefresh() {//����ˢ��ʱ����ô˷���
							//Log.i(LOG_TAG, "onRefresh called from SwipeRefreshLayout");

							// This method performs the actual data-refresh operation.
							// The method calls setRefreshing(false) when it's finished.
							updateOperation();
						}
					}
			);

			mRecyclerView = (RecyclerView) view.findViewById(R.id.my_recycler_view);

			// ���Բ��ֹ�������֧�ֺ����������ʽ
			mLayoutManager = new LinearLayoutManager(this.getActivity());
			mRecyclerView.setLayoutManager(mLayoutManager);

			// ����adapter
			myRecyclerViewAdapter = new RecyclerViewAdapter(mData);
			mRecyclerView.setAdapter(myRecyclerViewAdapter);
			myRecyclerViewAdapter.notifyDataSetChanged();

			return view;
		}


		// ��ʼ������
		private void initData(){
			mData = new ArrayList<>();
			mData.add(new Bean("��ʼ��������","content",R.drawable.bg));
		}

		//���²���
		public void updateOperation(){
			mData.add(new Bean("������������","content",R.drawable.bg));
			mSwipeRefreshLayout.setRefreshing(false);
		}
	}
	```

5. Ч����(���ڸ����ٶȽϿ죬������û�ص�)   
	![��ʼ��.png](https://ooo.0o0.ooo/2017/03/20/58cf8dd877f66.png) 
		
	![����.png](https://ooo.0o0.ooo/2017/03/20/58cf8dd98035d.png)



### �ο�����
- [Android Developer - Adding Swipe-to-Refresh To Your App](https://developer.android.com/training/swipe/add-swipe-interface.html#AddRefreshAction)   
- [Android Developer - �����б��뿨Ƭ](https://developer.android.com/training/material/lists-cards.html)   
- [Android Developer - API](https://developer.android.com/reference/packages.html)   
- [CSDN - Android������֮RecyclerView�ļ�ʹ�� ](http://blog.csdn.net/mr_dsw/article/details/48809429)

***