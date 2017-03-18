*author : xinfe*   
*date : 2017-03-18*
***
# ����Android(1)BottomNavigationBar��Fragment���ۺ�ʹ��

### һ������
ֱ����gradle��ץȡ��
![grab.PNG](https://ooo.0o0.ooo/2017/03/18/58cc99ec7dbba.png)  


### ����ʹ��([Usage](https://github.com/Ashok-Varma/BottomNavigation/wiki/Usage))

**�ڲ����ļ���д�룺**    
![xml�е�ʹ��.PNG](https://ooo.0o0.ooo/2017/03/18/58ccba779c1c4.png)   

**��Activity��д�룺**    
![activity�е�ʹ��.PNG](https://ooo.0o0.ooo/2017/03/18/58ccbaaa1fb62.png)   
 
**Ҳ��Ϊÿ��tab�����á�ѡ�С���δѡ�С�����ɫ��**   
```
bottomNavigationBar
    .addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setActiveColor(R.color.orange).setInActiveColor(R.color.teal))
    .addItem(new BottomNavigationItem(R.drawable.ic_book_white_24dp, "Books").setActiveColor("#FFFF00"))
    .addItem(new BottomNavigationItem(R.drawable.ic_music_note_white_24dp, "Music").setInActiveColor("#00FFFF"))
    .addItem(new BottomNavigationItem(R.drawable.ic_tv_white_24dp, "Movies & TV"))
    .addItem(new BottomNavigationItem(R.drawable.ic_videogame_asset_white_24dp, "Games").setActiveColor(R.color.grey))
```

**Ҳ�����ò�ͬ��ͼƬ(��δ���ʱ��һ��ͼƬ�����֮������һ��ͼƬ)**    
```
new BottomNavigationItem(R.drawable.ic_home_white_24dp, "Home").setInactiveIcon(R.drawable.ic_home_black)
```



### ����ʵ��
**activity_main.xml**
![layout.PNG](https://ooo.0o0.ooo/2017/03/18/58ccbad8ca519.png)

**MainActivity.java**
```
package team.xinfe.shiyu;

import android.app.Fragment;
import android.app.FragmentManager;
import android.app.FragmentTransaction;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;

import com.ashokvarma.bottomnavigation.BottomNavigationBar;
import com.ashokvarma.bottomnavigation.BottomNavigationItem;

import team.xinfe.shiyu.fragments.AddFragment;
import team.xinfe.shiyu.fragments.HomeFragment;
import team.xinfe.shiyu.fragments.MeFragment;

public class MainActivity extends AppCompatActivity implements BottomNavigationBar.OnTabSelectedListener{
    private static final String TAG="MainActivity";
    private Fragment homeFragment,addFragment,meFragment;

    protected void onCreate(Bundle savedInstanceState) {
        Log.d("shiyu", MainActivity.TAG+"onCreate");
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        BottomNavigationBar bottomNavigationBar = (BottomNavigationBar) findViewById(R.id.bottom_navigation_bar);

        bottomNavigationBar
		// Values:MODE_DEFAULT, MODE_FIXED, MODE_SHIFTING(��λ)
                .setMode(BottomNavigationBar.MODE_FIXED)   
		//Values: BACKGROUND_STYLE_DEFAULT, BACKGROUND_STYLE_STATIC, BACKGROUND_STYLE_RIPPLE(����)
                .setBackgroundStyle(BottomNavigationBar.BACKGROUND_STYLE_STATIC)
                .setActiveColor(R.color.colorPrimary)       //ѡ��ʱtab����ɫ
                .setInActiveColor(R.color.Black)            //δѡ��ʱtab����ɫ
                .setBarBackgroundColor(R.color.White)       //������ɫ
                .addItem(new BottomNavigationItem(R.drawable.ic_home_white_24dp, "��ҳ"))
                .addItem(new BottomNavigationItem(R.drawable.ic_add_white_24dp, "���"))
                .addItem(new BottomNavigationItem(R.drawable.ic_me_white_24dp, "�ҵ�"))
                .initialise();
        bottomNavigationBar.setTabSelectedListener(this);
        setDefaultFragment();
    }

    @Override
    public void onTabSelected(int position) {
        FragmentManager fm = this.getFragmentManager();
        FragmentTransaction transaction = fm.beginTransaction();
        switch (position) {
            case 0:
                if (homeFragment == null) {
                    homeFragment = new HomeFragment();
                }
                transaction.replace(R.id.fl_mainContent, homeFragment);
                break;
            case 1:
                if (addFragment == null) {
                    addFragment = new AddFragment();
                }
                transaction.replace(R.id.fl_mainContent, addFragment);
                break;
            case 2:
                if (meFragment == null) {
                    meFragment = new MeFragment();
                }
                transaction.replace(R.id.fl_mainContent, meFragment);
                break;
            default:
                break;
        }
        transaction.commit();
    }

    @Override
    public void onTabUnselected(int position) {

    }

    @Override
    public void onTabReselected(int position) {

    }

    //����Ĭ����ʾ��fragment
    private void setDefaultFragment(){
        Log.d("shiyu", MainActivity.TAG+"setDefaultFragment");
        FragmentManager fm = getFragmentManager();
        FragmentTransaction transaction = fm.beginTransaction();
        homeFragment = new HomeFragment();
        //transaction.add(R.id.fl_mainContent, homeFragment).show(homeFragment);
        transaction.replace(R.id.fl_mainContent, homeFragment);
        transaction.commit();
    }
}

```

### �ġ�ע��   
**v4����֧�ֵ�Fragment��app���µ�Fragment**   

v4����FragmentManager:
```
//3.0���°汾û��fragment��api,�������V4�������getSupportFragmentManager()��������ȡFragmentManager()����
//����getSupportFragmentManager()�������÷�Χ��ֻ���ڲ���activity�����á�
//������getSupportFragmentManager()û���������ʱ���޸�activityΪFragmentActivity����AppCompatActivity��

FragmentManager fragmentManager = getSupportFragmentManager();
```
app����FragmentManager:
``` 
//3.0�汾֮������Fragment��api���Ϳ���ֱ��ʹ��getFragmentManager()�����������ȡ��

Fragmentmanager fragmentManager = getFragmentManager();
```

**Fragment��replace()��hide(),add(),show()**   

����Ŀ���л�Fragment��һֱ������replace()�������滻Fragment��
������������һ�����⣬ÿ���л���ʱ��Fragment��������ʵ�л������¼���һ�����ݣ���������ǳ��������ܺ��û���������   

�ٷ��ĵ�����˵��replace()�������ֻ������һ��Fragment������Ҫʱ���õļ�㷽����   
�Ϻõ��л�������ʹ��hide,add,show������   

˼·�ǣ����һ����ť��Ҫ��ʾĳ��fragmentʱ��
�����жϸ�fragment�Ƿ��Ѿ�add����
���û��add����hide��ǰfragment��add��fragment��
�Ѿ�add���������Ѿ����ڸ�fragmentʵ����ֻ��hide��ǰ��ʾ��fragment��show��fragment��
���������������Fragment�л�������ʵ�����������ڴ治��ʱҲ����ɽ����ص����⡣   

**�ڴ治��Fragment�����ص��Ľ���취��**
- ״̬��⣺[Android Frament���л������replace�ĵ�Ч��](http://www.cnblogs.com/android-joker/p/4414891.html) [ fragment�����ص�������ռ�������� ](http://blog.csdn.net/fussenyu/article/details/51283282)
- ��ֹ���ݻع���[ fragment�ص����⣨add hide show��ʽ��](http://blog.csdn.net/fly7632785/article/details/48295741)



### �ο�����
[Github - BottomNavigation](https://github.com/Ashok-Varma/BottomNavigation)   
[Bottom navigation - Components - Material design guidelines](https://material.io/guidelines/components/bottom-navigation.html#bottom-navigation-specs)
***