*author : xinfe*   
*date : 2017-03-23*
***
# 关于Android(4)RecyclerView的点击事件处理

首先，查看[RecyclerView的API](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)，
发现它并没有 addOnItemClickListener 方法，只有 addOnItemTouchListener。     

### 方法一：利用 View.OnClickListener   

1. 按照对 ListView 的点击事件处理，是调用 AdapterView 的 setOnItemClickListener 方法（ListView继承了AdapterView）,如下：

	```java
	listview.setOnItemClickListener(new OnItemClickListener(){
	
		@Override
		public void onItemClick(AdapterView<?> parent, View view, int position,long id) {
			Toast.makeText(this,"点击了一下",Toast.LENGTH_SHORT).show();
		}     
		
	});
	```

2. 我们也希望可以和 ListView 一样，但问题在于 Item ，想到之前定义了一个 RecyclerViewAdapter ，在这里面有对 Item 的处理，所以我们希望此类提供了一个方法 setOnItemClickListener()：

	```java
	public void setOnItemClickListener(OnItemClickListener onItemClickListener){
		this.mOnItemClickListener = onItemClickListener;	//mOnItemClickListener是RecyclerViewAdapter的私有成员变量
	}
	```

	其中 OnItemClickListener 是 RecyclerViewAdapter 的一个内部接口：
	```java
	public interface OnItemClickListener{
		void onItemClick(View view,int position);//此方法由用户实现
	}
	```

3. 这样我们就可以在 Activity 或者 Fragment 中调用 RecyclerViewAdapter 的 setOnItemClickListener 方法。

	```java
	RecyclerViewAdapter = new RecyclerViewAdapter(mData);
	RecyclerViewAdapter.setOnItemClickListener(new RecyclerViewAdapter.OnItemClickListener() {
		@Override
		public void onItemClick(View view, int position) {
			Toast.makeText(getActivity(), "click " + mData.get(position), Toast.LENGTH_SHORT).show();
		}
	});
	```

	由此门面就做好了。    

4. 接下来要真正处理 RecyclerViewAdapter 中对 Item 的点击处理了。回到 RecyclerViewAdapter 中，在 onBindViewHolder 方法内判断用户是否设置了监听器，如果设置了，就为当前的 itemView 设置 View.OnClickListener ，在onClick方法中实现回调：
	```java
	@Override
	public void onBindViewHolder(final ViewHolder holder, int position) {
		holder.mTextView.setText(mDataSet.get(position));
		//判断是否设置了监听器
		if(mOnItemClickListener != null){
			//为ItemView设置监听器
			holder.itemView.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
					mOnItemClickListener.onItemClick(holder.itemView,holder.getLayoutPosition());
				}
			});
		}
	```
	这样就能实现对 RecyclerView 的 Item 进行点击处理了。   

5. 这种方法的大致过程就是：   
	我们希望可以和 ListView 一样调用 setOnItemClickListener 方法，但 RecyclerView 没有提供这种方法。   
	要对 RecyclerView 的 ItemView 进行点击处理，我们想到了 RecyclerViewAdapter ，因为我们可以在它的 onBindViewHolder 方法中对 ItemView 进行处理。   
	当我们获得了 ItemView ，就可以对它设置 View.OnClickListener ，在 View.OnClickListener 的 onClick 方法中回调我们自己设置的 onItemClick 方法即可。

6. 缺点：   
	这种方法将监听器设置在了 RecyclerViewAdapter 中，我们调用的是 recyclerViewAdapter.setOnItemClickListener 。 而不是 recyclerView.setOnItemClickListener。监听器与适配器耦合度太高。   
	而且，其中涉及到了两种监听器，一种是我们自己定义的 RecyclerViewAdapter.OnItemClickListener ，另一种是 View.OnClickListener 。    
	所以，不是很推荐使用。


### 方法二：利用 RecyclerView.OnItemTouchListener 和 GestureDetector(手势检测类)       
1. 新建一个 RecyclerViewItemClickListener 实现 RecyclerView.OnItemTouchListener ：
	```java
	package team.xinfe.shiyu.listener;

	import android.content.Context;
	import android.support.v7.widget.RecyclerView;
	import android.view.GestureDetector;
	import android.view.MotionEvent;
	import android.view.View;

	/**
	 * RecyclerViewItemClickListener
	 * 实现RecyclerView.OnItemTouchListener接口
	 * 再声明一个内部接口，由用户实现不同的点击RecyclerViewItem的处理方式
	 * Created by xinfe on 2017/3/22.
	 */

	public class RecyclerViewItemClickListener implements RecyclerView.OnItemTouchListener {
		private OnItemClickListener mOnItemClickListener;
		private GestureDetector mGestureDetector;


		//内部接口，定义点击方法，由用户实现
		public interface OnItemClickListener {

			void onItemClick(View view, int position);

		}


		public RecyclerViewItemClickListener(Context context, final RecyclerView recyclerView, OnItemClickListener listener){
			mOnItemClickListener = listener;
			mGestureDetector = new GestureDetector(context,
					new GestureDetector.SimpleOnGestureListener(){ //这里选择SimpleOnGestureListener实现类，可以根据需要选择重写的方法
						//单击事件
						@Override
						public boolean onSingleTapUp(MotionEvent e) {
							View childView = recyclerView.findChildViewUnder(e.getX(),e.getY());//获取事件发生处的ItemView
							if(childView != null && mOnItemClickListener != null){
								mOnItemClickListener.onItemClick(childView,recyclerView.getChildLayoutPosition(childView));
								return true;
							}
							return false;
						}
					});
		}



		@Override
		public boolean onInterceptTouchEvent(RecyclerView rv, MotionEvent e) {
			//把事件交给GestureDetector处理
			if(mGestureDetector.onTouchEvent(e)){
				return true;
			}
			return false;
		}

		@Override
		public void onTouchEvent(RecyclerView rv, MotionEvent e) {

		}

		@Override
		public void onRequestDisallowInterceptTouchEvent(boolean disallowIntercept) {

		}
	}

	```
	
2. 在 Activity 或 Fragment 中调用(此处是 Fragment)：   
	```java
        mRecyclerView.addOnItemTouchListener(new RecyclerViewItemClickListener(getActivity(), mRecyclerView,
                new RecyclerViewItemClickListener.OnItemClickListener() {
                    @Override
                    public void onItemClick(View view, int position) {
                        Log.d("shiyu",TAG+"onItemClick");
                        Toast.makeText(getActivity(),"hahaha",Toast.LENGTH_SHORT).show();
                    }
                }));
	```

3. 大体流程就是：   
	在调用 RecyclerView 的 addOnItemTouchListener 时，完成 RecyclerViewItemClickListener 的初始化，   
	然后就一直监听 RecyclerView 的事件，并将事件交给 GestureDetector 处理，   
	如果遇到了 onSingleTapUp 就获得当前位置，获得ItemView，再回调自己定义好的方法，处理事件。   
	
4. 效果：   
	![RecyclerView的点击处理.png](https://ooo.0o0.ooo/2017/03/23/58d3640a88620.png)



### 参考资料
- [揭开RecyclerView的神秘面纱(二)：处理RecyclerView的点击事件 ](http://blog.csdn.net/zchlww/article/details/51525579)   
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)   
- [RecyclerView.OnItemTouchListener](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.OnItemTouchListener.html)   
***