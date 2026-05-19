---
title: '[MaterialDesign] Android Tabs'
date: 2018-01-28 22:39:48
tags: ['Android','Material Design']
---

## Introduction
在**Material Design**中，**Tabs**使用頁籤的方式管理不同的畫面，通常位於**Toolbar的下方，**常與**Fragment/ViewPager**搭配使用。<!-- more -->背景的顏色通常也與**Toolbar**一致
<img src="tabs01.png" width=50% height=50% align=center/>

在常見的使用中，**Tabs**可以被分為兩種模式
**1.Fixed**
在**Fixed**模式下，每個**Tab item**的大小都是一致的，也因為大小都一樣，所以在設計上通常放置兩個(含)以上，最多四個**item**
**2.Scrollable**
在**Scrollable**模式下，**Tab item**常為4個以上

在**Tabs item**設計上，不管是**Fixed**模式或是**Scrollable**模式，都可以用**文字 / 圖案 / 文字+圖案**下去設計，接下來我們來實際創建一個**Tabs + ViewPager**

## Create a Tab Layout with Text: Fixed Mode

### 添加support:design library
使用**TabLayout**，首先需要把**support:design library**添加進我們的**Project**中，在 File -> Project Structure -> Dependencies 下新增。
<img src="tabs02.png" width=100% height=100% align=center/>

### 創建TabLayout
在**xml**裡，創建一個**TabLayout**，賦予屬性**app:tabMode="fixed"**，背景顏色使用跟**Toolbar(colorPrimary)**顏色一致，接著在下方創建**ViewPager**

**app:tabGravity="fill"**表示tabs item會填滿整個**TabLayout**

layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

   <android.support.v7.widget.Toolbar
       android:layout_width="match_parent"
       android:layout_height="?attr/actionBarSize"
       android:id="@+id/toolbar_id"
       android:background="@color/colorPrimary"
       app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>

   <android.support.design.widget.TabLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/tabLayout_id"
       android:layout_below="@+id/toolbar_id"
       android:background="@color/colorPrimary"
       app:tabMode="fixed"
       app:tabGravity="fill"/>

   <android.support.v4.view.ViewPager
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:id="@+id/viewPager_id"
       android:layout_below="@+id/tabLayout_id"/>

</RelativeLayout>
```

預覽效果如下:
<img src="tabs03.png" width=50% height=50% align=center/>

### Define layouts for the items in TabLayout
我們新增**Fragment**的**layout**，這些**View**，會顯示在**ViewPager**中，同時透過**TabLayout**的**setupWithViewPager()**方法，能夠自動產生對應**ViewPager**內**view**的**tab item**。

新增**Fragment layout**
layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.v7.widget.AppCompatTextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="@string/main_item_one"
        android:textSize="60sp"
        android:textStyle="bold"/>

</RelativeLayout>
```

預覽效果如下:
<img src="tabs04.png" width=50% height=50% align=center/>

接著依序創建**layout_item_two**與**layout_item_three_layout**，總共3個**layout**
<img src="tabs05.png" width=50% height=50% align=center/>

### Create Java class for item layout
接下來創建**Fragment class**

java:
``` bash
public class OneFragment extends Fragment {

    private View view;

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
       view = inflater.inflate(R.layout.layout_item_one, container, false);
       return view;
    }
}
```

依序創建：
**layout_item_one**
**layout_item_two**
**layout_item_three**
的**Fragment class**
<img src="tabs06.png" width=50% height=50% align=center/>

### Create a ViewPager Adapter
接下來我們創建**ViewPagerAdapter**，並繼承**FragmentPagerAdapter**

java:
``` bash
public class ViewPagerAdapter extends FragmentPagerAdapter {

    private List<Fragment> fragmentList = new ArrayList<>();
    private List<String> fragmentTitleList = new ArrayList<>();

    public ViewPagerAdapter(FragmentManager fm) {
        super(fm);
    }

    @Override
    public Fragment getItem(int position) {
        // Fragment
        return fragmentList.get(position);
    }

    @Override
    public int getCount() {
        return fragmentList.size();
    }

    @Nullable
    @Override
    public CharSequence getPageTitle(int position) {
        // 取得當前頁的title
        return fragmentTitleList.get(position);
    }

    // 添加Fragment 與 Title 方法
    public void addFragment(Fragment fragment, String title) {
        fragmentList.add(fragment);
        fragmentTitleList.add(title);
    }
}
```

### Setting TabLayout
在**Adapter**創建完成之後，回到**Activity**，把**Fragment**加進**adapter**，並把它與**ViewPager**綁定

java:
``` bash
ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());

adapter.addFragment(new OneFragment(), "ITEM ONE");
adapter.addFragment(new TwoFragment(), "ITEM TWO");
adapter.addFragment(new ThreeFragment(), "ITEM THREE");

viewPager.setAdapter(adapter);
```

**TabLayout**有提供我們一個基於**ViewPager**快速創建對應**Tabs**的方法

java:
``` bash
tabLayout = findViewById(R.id.tabLayout_id);

tabLayout.setupWithViewPager(viewPager);
```

運行效果如下：
<img src="tabs07.gif" width=50% height=50% align=center/>

改變**Tab**文字顏色，在**TabLayout**的**tag**裡，添加屬性
``` bash
app:tabTextColor="@color/colorWhite"
```
運行效果如下：
<img src="tabs08.gif" width=50% height=50% align=center/>

改變**Tab**被選中的文字顏色，在**TabLayout**的**tag**裡，添加屬性
``` bash
 app:tabSelectedTextColor="@color/colorAccent"
```
運行效果如下：
<img src="tabs09.gif" width=50% height=50% align=center/>

改變**TabLayout**下方游標顏色，在**TabLayout**的**tag**裡，添加屬性
``` bash
app:tabIndicatorColor="@color/colorLightBlue"
```
運行效果如下：
<img src="tabs16.png" width=50% height=50% align=center/>

改變**TabLayout**下方游標高度，在**TabLayout**的**tag**裡，添加屬性
``` bash
app:tabIndicatorHeight="10dp"
```
運行效果如下：
<img src="tabs17.png" width=50% height=50% align=center/>


## Create a Tab Layout with Text: Scrollable Mode
要創建可滑動的**TabLayout**，首先把屬性**app:tabMode**改為**scrollable**
``` bash
app:tabMode="scrollable"
```
``` bash
 <android.support.design.widget.TabLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/tabLayout_id"
       android:layout_below="@+id/toolbar_id"
       android:background="@color/colorPrimary"
       app:tabMode="scrollable"
       app:tabGravity="fill"
       app:tabTextColor="@android:color/white"
       app:tabSelectedTextColor="@color/colorAccent"/>
```

接著創建十個**layout**
<img src="tabs10.png" width=50% height=50% align=center/>

創建十個**Java Class**
<img src="tabs11.png" width=50% height=50% align=center/>

在**Activity**裡添加
``` java
ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
adapter.addFragment(new OneFragment(), "ITEM ONE");
adapter.addFragment(new TwoFragment(), "ITEM TWO");
adapter.addFragment(new ThreeFragment(), "ITEM THREE");
adapter.addFragment(new FourFragment(), "ITEM FOUR");
adapter.addFragment(new FiveFragment(), "ITEM FIVE");
adapter.addFragment(new SixFragment(), "ITEM SIX");
adapter.addFragment(new SevenFragment(), "ITEM SEVEN");
adapter.addFragment(new EightFragment(), "ITEM EIGHT");
adapter.addFragment(new NineFragment(), "ITEM NINE");
adapter.addFragment(new TenFragment(), "ITEM TEN");
viewPager.setAdapter(adapter);

tabLayout.setupWithViewPager(viewPager);
```

運行效果如下：
<img src="tabs18.gif" width=50% height=50% align=center/>


## Create a Tab Layout with Icon
接下來我們以**Fixed Mode**實現，在**TabLayout**頁籤裡，只顯示**icon**的效果

首先在**XML**裡，把**app:tabMode**設為**"fixed"**
``` bash
<android.support.design.widget.TabLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/tabLayout_id"
       android:layout_below="@+id/toolbar_id"
       android:background="@color/colorPrimary"
       app:tabMode="fixed"
       app:tabGravity="fill"
       app:tabTextColor="@android:color/white"
       app:tabSelectedTextColor="@color/colorAccent"/>
```

接著同樣新增3個**Layout**，
``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/recents"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:textSize="40sp"
        android:textStyle="bold"/>
    
</RelativeLayout>
```
<img src="tabs12.png" width=70% height=70% align=center/>

創建剛剛新增**layout**的**java class(Fragment)**
``` java
public class RecentsFragment extends Fragment {

    View view;

    public RecentsFragment() {}
    
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        view = inflater.inflate(R.layout.recents_layout,container,false);
        return view;
    }
}
```
<img src="tabs13.png" width=70% height=70% align=center/>

創建**ViewPagerAdapter**，這邊我們沒有覆寫**getPageTitle()**方法
``` java
public class ViewPagerAdapter extends FragmentPagerAdapter {

    private List<Fragment> fragmentList = new ArrayList<>();
    private List<String> fragmentTitleList = new ArrayList<>();

    public ViewPagerAdapter(FragmentManager fm) {
        super(fm);
    }

    @Override
    public Fragment getItem(int position) {
        // Fragment
        return fragmentList.get(position);
    }

    @Override
    public int getCount() {
        return fragmentList.size();
    }

    // 添加Fragment 與 Title 方法
    public void addFragment(Fragment fragment, String title) {
        fragmentList.add(fragment);
        fragmentTitleList.add(title);
    }
}
```

在**Activity**裡初始化，在綁定**Adapter**後，使用**getTabAt()**取得各項**Tab**後，設定各個**icon**
``` java
public class MainActivity extends AppCompatActivity {

    Toolbar toolbar;
    TabLayout tabLayout;
    ViewPager viewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        toolbar = findViewById(R.id.toolbar_id);
        setSupportActionBar(toolbar);

        viewPager = findViewById(R.id.viewPager_id);
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFragment(new RecentsFragment(),"RECENTS");
        adapter.addFragment(new FavoritesFragment(),"FAVORITES");
        adapter.addFragment(new NearbyFragment(),"NEARBY");
        viewPager.setAdapter(adapter);

        tabLayout = findViewById(R.id.tabLayout_id);
        tabLayout.setupWithViewPager(viewPager);
        // 添加icon
        tabLayout.getTabAt(0).setIcon(R.drawable.phone);
        tabLayout.getTabAt(1).setIcon(R.drawable.heart);
        tabLayout.getTabAt(2).setIcon(R.drawable.account);
    }
}
```

運行效果如下：
<img src="tabs14.gif" width=50% height=50% align=center/>

## Create a Tab Layout with Icon and Text
如果想要**Tabs**同時擁有**圖片**和**文字**，只要在**ViewPageAdapter**裡，覆寫**getPageTitle()**方法就行了。
``` java
public class ViewPagerAdapter extends FragmentPagerAdapter {

    private List<Fragment> fragmentList = new ArrayList<>();
    private List<String> fragmentTitleList = new ArrayList<>();

    public ViewPagerAdapter(FragmentManager fm) {
        super(fm);
    }

    @Override
    public Fragment getItem(int position) {
        // Fragment
        return fragmentList.get(position);
    }

    @Override
    public int getCount() {
        return fragmentList.size();
    }

    @Nullable
    @Override
    public CharSequence getPageTitle(int position) {
        // 取得當前頁的title
        return fragmentTitleList.get(position);
    }

    // 添加Fragment 與 Title 方法
    public void addFragment(Fragment fragment, String title) {
        fragmentList.add(fragment);
        fragmentTitleList.add(title);
    }
}
```

運行效果如下：
<img src="tabs15.gif" width=50% height=50% align=center/>

## Create a Tabs item with Java
不使用**ViewPager**，也可以透過代碼創建**TabsItem**
``` java
tabLayout = findViewById(R.id.tabLayout_id);

tabLayout.addTab(tabLayout.newTab().setText("RECENTS").setIcon(R.drawable.phone));
tabLayout.addTab(tabLayout.newTab().setText("FAVORITES").setIcon(R.drawable.heart));
tabLayout.addTab(tabLayout.newTab().setText("NEARBY").setIcon(R.drawable.account));
```

## Create a Tabs item in XML
不使用**ViewPager**，在**Layout.xml**裡，創建**TabItem**
``` bash
<android.support.design.widget.TabLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/tabLayout_id"
       android:layout_below="@+id/toolbar_id"
       android:background="@color/colorPrimary"
       app:tabMode="fixed"
       app:tabGravity="fill">

    <android.support.design.widget.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/recents"
        android:icon="@drawable/phone"/>

    <android.support.design.widget.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/favorites"
        android:icon="@drawable/heart"/>

    <android.support.design.widget.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/nearby"
        android:icon="@drawable/account"/>

</android.support.design.widget.TabLayout>
```


參考資料：
[Android TabLayout](https://developer.android.com/reference/android/support/design/widget/TabLayout.html)
[Material Design](https://material.io/guidelines/components/tabs.html)