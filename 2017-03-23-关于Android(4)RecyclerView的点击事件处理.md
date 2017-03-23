*author : xinfe*   
*date : 2017-03-23*
***
# ����Android(4)RecyclerView�ĵ���¼�����

���ȣ��鿴[RecyclerView��API](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)��
��������û�� addOnItemClickListener ������ֻ�� addOnItemTouchListener��     

### ����һ������ View.OnClickListener   

1. ���ն� ListView �ĵ���¼������ǵ��� AdapterView �� setOnItemClickListener ������ListView�̳���AdapterView��,���£�

	```java
	listview.setOnItemClickListener(new OnItemClickListener(){
	
		@Override
		public void onItemClick(AdapterView<?> parent, View view, int position,long id) {
			Toast.makeText(this,"�����һ��",Toast.LENGTH_SHORT).show();
		}     
		
	});
	```

2. ����Ҳϣ�����Ժ� ListView һ�������������� Item ���뵽֮ǰ������һ�� RecyclerViewAdapter �����������ж� Item �Ĵ�����������ϣ�������ṩ��һ������ setOnItemClickListener()��

	```java
	public void setOnItemClickListener(OnItemClickListener onItemClickListener){
		this.mOnItemClickListener = onItemClickListener;	//mOnItemClickListener��RecyclerViewAdapter��˽�г�Ա����
	}
	```

	���� OnItemClickListener �� RecyclerViewAdapter ��һ���ڲ��ӿڣ�
	```java
	public interface OnItemClickListener{
		void onItemClick(View view,int position);//�˷������û�ʵ��
	}
	```

3. �������ǾͿ����� Activity ���� Fragment �е��� RecyclerViewAdapter �� setOnItemClickListener ������

	```java
	RecyclerViewAdapter = new RecyclerViewAdapter(mData);
	RecyclerViewAdapter.setOnItemClickListener(new RecyclerViewAdapter.OnItemClickListener() {
		@Override
		public void onItemClick(View view, int position) {
			Toast.makeText(getActivity(), "click " + mData.get(position), Toast.LENGTH_SHORT).show();
		}
	});
	```

	�ɴ�����������ˡ�    

4. ������Ҫ�������� RecyclerViewAdapter �ж� Item �ĵ�������ˡ��ص� RecyclerViewAdapter �У��� onBindViewHolder �������ж��û��Ƿ������˼���������������ˣ���Ϊ��ǰ�� itemView ���� View.OnClickListener ����onClick������ʵ�ֻص���
	```java
	@Override
	public void onBindViewHolder(final ViewHolder holder, int position) {
		holder.mTextView.setText(mDataSet.get(position));
		//�ж��Ƿ������˼�����
		if(mOnItemClickListener != null){
			//ΪItemView���ü�����
			holder.itemView.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
					mOnItemClickListener.onItemClick(holder.itemView,holder.getLayoutPosition());
				}
			});
		}
	```
	��������ʵ�ֶ� RecyclerView �� Item ���е�������ˡ�   

5. ���ַ����Ĵ��¹��̾��ǣ�   
	����ϣ�����Ժ� ListView һ������ setOnItemClickListener �������� RecyclerView û���ṩ���ַ�����   
	Ҫ�� RecyclerView �� ItemView ���е�����������뵽�� RecyclerViewAdapter ����Ϊ���ǿ��������� onBindViewHolder �����ж� ItemView ���д���   
	�����ǻ���� ItemView ���Ϳ��Զ������� View.OnClickListener ���� View.OnClickListener �� onClick �����лص������Լ����õ� onItemClick �������ɡ�

6. ȱ�㣺   
	���ַ������������������� RecyclerViewAdapter �У����ǵ��õ��� recyclerViewAdapter.setOnItemClickListener �� ������ recyclerView.setOnItemClickListener������������������϶�̫�ߡ�   
	���ң������漰�������ּ�������һ���������Լ������ RecyclerViewAdapter.OnItemClickListener ����һ���� View.OnClickListener ��    
	���ԣ����Ǻ��Ƽ�ʹ�á�


### ������������ RecyclerView.OnItemTouchListener �� GestureDetector(���Ƽ����)       
1. �½�һ�� RecyclerViewItemClickListener ʵ�� RecyclerView.OnItemTouchListener ��
	```java
	package team.xinfe.shiyu.listener;

	import android.content.Context;
	import android.support.v7.widget.RecyclerView;
	import android.view.GestureDetector;
	import android.view.MotionEvent;
	import android.view.View;

	/**
	 * RecyclerViewItemClickListener
	 * ʵ��RecyclerView.OnItemTouchListener�ӿ�
	 * ������һ���ڲ��ӿڣ����û�ʵ�ֲ�ͬ�ĵ��RecyclerViewItem�Ĵ���ʽ
	 * Created by xinfe on 2017/3/22.
	 */

	public class RecyclerViewItemClickListener implements RecyclerView.OnItemTouchListener {
		private OnItemClickListener mOnItemClickListener;
		private GestureDetector mGestureDetector;


		//�ڲ��ӿڣ����������������û�ʵ��
		public interface OnItemClickListener {

			void onItemClick(View view, int position);

		}


		public RecyclerViewItemClickListener(Context context, final RecyclerView recyclerView, OnItemClickListener listener){
			mOnItemClickListener = listener;
			mGestureDetector = new GestureDetector(context,
					new GestureDetector.SimpleOnGestureListener(){ //����ѡ��SimpleOnGestureListenerʵ���࣬���Ը�����Ҫѡ����д�ķ���
						//�����¼�
						@Override
						public boolean onSingleTapUp(MotionEvent e) {
							View childView = recyclerView.findChildViewUnder(e.getX(),e.getY());//��ȡ�¼���������ItemView
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
			//���¼�����GestureDetector����
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
	
2. �� Activity �� Fragment �е���(�˴��� Fragment)��   
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

3. �������̾��ǣ�   
	�ڵ��� RecyclerView �� addOnItemTouchListener ʱ����� RecyclerViewItemClickListener �ĳ�ʼ����   
	Ȼ���һֱ���� RecyclerView ���¼��������¼����� GestureDetector ����   
	��������� onSingleTapUp �ͻ�õ�ǰλ�ã����ItemView���ٻص��Լ�����õķ����������¼���   
	
4. Ч����   
	![RecyclerView�ĵ������.png](https://ooo.0o0.ooo/2017/03/23/58d3640a88620.png)



### �ο�����
- [�ҿ�RecyclerView��������ɴ(��)������RecyclerView�ĵ���¼� ](http://blog.csdn.net/zchlww/article/details/51525579)   
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)   
- [RecyclerView.OnItemTouchListener](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.OnItemTouchListener.html)   
***