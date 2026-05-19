---
title: '[Material Design] Android Navigation Drawer(一)'
date: 2017-12-30 14:26:14
tags: ['Android','Material Design']
---

## Introduction

Navigation Drawer 是一個隱藏在 APP 畫面左邊的面板，通常會用在應用的主頁，用以顯示應用主要的功能項，預設情況下通常是隱藏在左邊，使用者可以透過手指，在螢幕上由左向右滑動，把 Navigation drawer 呼叫顯示出來。
<!-- more -->
可以先參考[Material Design](https://material.io/guidelines/patterns/navigation-drawer.html#navigation-drawer-behavior)官網的圖片
<img src="navigationDrawer01.png" width=100% height=100% align=center/>

通常 Navigation Drawer 可以由幾部分組成：

### Header
Headers 通常用來顯示圖片，或聯絡資訊，當然這並不是所有的 APP 都這樣做

### Navigation View
在 Header 下面的區塊，稱為 Navigation View ，我們可以自定義 items 包含 icon 在這裡，這些 items 將會組成我們的 Navigation Drawer Munu
<img src="navigationDrawer02.png" width=50% height=50% align=center/>

## Create a Navigation Drawer

在創建 Activity 時，Android Studio 有提供我們快速創建 Navigation Drawer Activity ，不過在本篇文章中，我們選擇 Empty Activity 創建
<img src="navigationDrawer03.png" width=100% height=100% align=center/>

### Add Design Library to Our Project
在創建 Navigation Drawer 的第一步，我們必須先確認此專案有 import Design Library，可以在 File -> Project Structure -> Dependencies 下新增。
<img src="navigationDrawer04.png" width=100% height=100% align=center/>

新增完在 build.gradle 下，注意版本需與 appcompat-v7 的版本一致
<img src="navigationDrawer05.png" width=100% height=100% align=center/>

### Remove default toolbar form application
進入 res -> values 目錄下，打開 style.xml file，把 AppTheme 改為

``` bash
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

</resources>
```
此舉的目的是，移除 App 預設的 toolbar，緊接著我們在 activity layout 使用自定義的 toolbar

``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="@color/colorPrimary"
        app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>

</android.support.constraint.ConstraintLayout>
```

回到 activity 中，使用 setSupportActionBar method 設定替換成自定義的 toolbar

``` bash
public class MainActivity extends AppCompatActivity {

    Toolbar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }
}
```

執行後可以看到效果
<img src="navigationDrawer06.png" width=50% height=50% align=center/>

### Create Drawer Layout

在這邊我們要創建一個新 layout，用以創建 Navigation Drawer，並把根佈局替換成，support.v4 包下面的 DrawerLayout
<img src="navigationDrawer07.png" width=100% height=100% align=center/>

緊接著我們在 DrawerLayout 添加屬性 id 與 fitsSystemWindows = true。
fitsSystemWindows 為 true 代表你的 DrawerLayout size 會與你的 Activity size 一致

``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/drawer_layout"
    android:fitsSystemWindows="true">

</android.support.v4.widget.DrawerLayout>
```

然後我們再 DrawerLayout 裡面，引用 Design Library 中的 Navigation View

``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/drawer_layout"
    android:fitsSystemWindows="true">

    <android.support.design.widget.NavigationView
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:id="@+id/navigation_view"
        android:layout_gravity="start"
        android:fitsSystemWindows="true" />

</android.support.v4.widget.DrawerLayout>
```
其中，layout_gravity="start" 表示你的 NavigationView 會位於 Activity 的左側。

接下來我們要把定義好的 DrawerLayout 設為 Activity 預設的 Layout，這表示當 Application 啟動 此 activity 頁面時，此 DrawerLayout 將會被預加載。

回到我們的 Activity，將 setContentView Method 中的 layout 替換成剛剛新增的 navigation_drawer.xml

``` bash
public class MainActivity extends AppCompatActivity {

    Toolbar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 這邊替換成 navigation_drawer
        setContentView(R.layout.navigation_drawer);

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }
}
```

然後再回到 navigation_drawer.xml 下，把原本的 activity_main.xml 用 include tag import 進來

``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/drawer_layout"
    android:fitsSystemWindows="true">

    <include layout="@layout/activity_main"/>

    <android.support.design.widget.NavigationView
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:id="@+id/navigation_view"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"/>

</android.support.v4.widget.DrawerLayout>
```

到了這邊大家可能會有點搞混，目前的畫面結構大概如下圖所示
<img src="navigationDrawer08.png" width=100% height=100% align=center/>

### Create Navigation Drawers Header
接下來我們創建，Navigation Drawer Header Layout 文件
<img src="navigationDrawer09.png" width=70% height=70% align=center/>

Navigation Header 的高度通常是介於 160 ~ 180 dp 之間，接下來我們把 Header 高度定義為 160 dp，並加入 Header Background，當然你也可以放入任何你喜歡圖
Header Layout 如下：
<img src="navigationDrawer10.png" width=100% height=100% align=center/>

### Add Navigation Drawers Menu
我們在完成 Header Layout 後，緊接著我們需要在 res -> menu 下創建 navigation_menu.xml，然後在此 menu 中定義我們的 item
<img src="navigationDrawer11.png" width=100% height=100% align=center/>

先在 group tag 裡 定義數個 item，checkableBehavior 表示點擊行為的效果反饋，緊接著再完成 group tag 後，新增 item tag 並在裡面再新增一個 menu tag 與裡面的 item 如下：

``` bash
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <group android:checkableBehavior="single">
        <item
            android:id="@+id/inbox_id"
            android:icon="@drawable/inbox"
            android:title="@string/inbox" />
        <item
            android:id="@+id/starred_id"
            android:icon="@drawable/starred"
            android:title="@string/starred" />
        <item
            android:id="@+id/sent_id"
            android:icon="@drawable/sent"
            android:title="@string/sent_mail" />
        <item
            android:id="@+id/drafts_id"
            android:icon="@drawable/drafts"
            android:title="@string/drafts" />
    </group>

    <item android:title="@string/subheader">
        <menu>
            <item
                android:id="@+id/allmail_id"
                android:icon="@drawable/allmail"
                android:title="@string/all_mail" />
            <item
                android:id="@+id/trash_id"
                android:icon="@drawable/trash"
                android:title="@string/trash" />
            <item
                android:id="@+id/spam_id"
                android:icon="@drawable/spam"
                android:title="@string/spam" />
        </menu>
    </item>
</menu>
```

再定義完 Navigation Drawers Menu 後，我們把回到 DrawerLayout，並把剛剛定義好的 Header 與 Menu 都加進，NavigationView 的屬性中
<img src="navigationDrawer12.png" width=100% height=100% align=center/>

緊接著我們回到 Activity 完善以下代碼
``` bash
public class MainActivity extends AppCompatActivity {

    Toolbar toolbar;
    DrawerLayout drawerLayout;
    NavigationView navigationView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.navigation_drawer);

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        drawerLayout = findViewById(R.id.drawer_layout);
        navigationView = findViewById(R.id.navigation_view);

        ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(this, drawerLayout, toolbar, R.string.open_drawer, R.string.close_drawer);
        drawerLayout.addDrawerListener(toggle);
        toggle.syncState();
    }
}
```

然後就可以在畫面上看到效果了
<img src="navigationDrawer13.gif" width=50% height=50% align=center/>




參考網站:
[Android Developer](https://developer.android.com/training/implementing-navigation/nav-drawer.html)
[Material Design](https://material.io/guidelines/patterns/navigation-drawer.html)