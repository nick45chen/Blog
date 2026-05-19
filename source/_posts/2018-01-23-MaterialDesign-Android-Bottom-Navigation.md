---
title: '[MaterialDesign] Android Bottom Navigation'
date: 2018-01-23 09:41:42
tags: ['Android','Material Design']
---

## Introduction
**Bottom Navigation**在**Material Design中**算是相當重要的一個組件，通常會用在主頁底部的部分<!-- more -->，在使用**Bottom Design**作為**App**設計上，有點要注意：

1.**Bottom Navigation**可以使用文字搭配圖示
2.通常放置3個**item**，最多不超過5個，如果少於2個**item**，請使用**Tabs**，超過5個，可以使用**Navigation drawers** 
3.選中狀態下的顏色，建議使用**App's primary color**或是同**Toolbar**的顏色，
4.如果**Bottom Navigation**已經有背景顏色，那選中狀態的文字與圖示應使用白色或是黑色
5.**Bottom Navigation**上的文字，盡量簡短明暸，以免發生文字斷行、縮小、未顯示的情形

接下來我們就來實作**Bottom Navigation**

## Create a Bottom Navigation
在**Android Studio**中，也提中快速創建**Bottom Navigation**的模板，不過我們本篇會選用**Empty Activity**
<img src="bottom_navigation01.png" width=100% height=100% align=center/>

1.要使用**BottomNavigationView**，首先需要把**support:design library**添加進我們的**Project**中，在 File -> Project Structure -> Dependencies 下新增。
<img src="bottom_navigation02.png" width=100% height=100% align=center/>

2.接下來在**layout**裡，創建一個**BottomNavigationView**，背景顏色使用跟窗口背景顏色一樣

layout:
``` bash
 <android.support.design.widget.BottomNavigationView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?android:attr/windowBackground"/>
```

3.緊接著我們要創建**BottomNavigationView** 的 **menu**，在**res -> menu**目錄下創建

menu:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:title="@string/recents"
        android:id="@+id/main_recents"
        android:icon="@drawable/ic_recents_white_24dp"
        android:orderInCategory="1"/>
    <item
        android:title="@string/favorites"
        android:id="@+id/main_favorites"
        android:icon="@drawable/ic_favorite_white_24dp"
        android:orderInCategory="2"/>
    <item
        android:title="@string/nearby"
        android:id="@+id/main_nearby"
        android:icon="@drawable/ic_near_by_white_24dp"
        android:orderInCategory="3"/>
</menu>
```

4.回到**layout**在**BottomNavigationView**的屬性中添加**menu**

layout:
``` bash
 <android.support.design.widget.BottomNavigationView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?android:attr/windowBackground"
        app:menu="@menu/bottom_navigation_menu"/>
```

運行效果如下：
<img src="bottom_navigation03.gif" width=50% height=50% align=center/>

## Customize the Bottom Navigation
接下來我們要改變**BottomNavigationView**被選中文字/圖示顏色，包含**BottomNavigationView**的背景。

**BottomNavigationView**預設選中的顏色是當前**Theme**中的**color primary**，也就是跟**Toolbar**顏色一致

1.首先建立一個新**style**，並自定義**color primary**的顏色

style:
``` bash
<style name="BottomNavigationTheme" parent="Widget.Design.BottomNavigationView">
    <item name="colorPrimary">@color/colorBottomNavigation</item>
</style>
```
2.回到**layout**在**BottomNavigationView**的屬性中添加上剛剛創建的**style**

layout:
``` bash
 <android.support.design.widget.BottomNavigationView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?android:attr/windowBackground"
        app:menu="@menu/bottom_navigation_menu"
        app:theme="@style/BottomNavigationTheme"/>
```

運行效果如下：
<img src="bottom_navigation04.gif" width=50% height=50% align=center/>

變更**BottomNavigationView**的背景圖案可以加入屬性：
``` bash
app:itemBackground="@color/colorCustomize"
```

變更**BottomNavigationView**的圖示顏色可以加入屬性：
``` bash
app:itemIconTint="@color/colorCustomize"
```

變更**BottomNavigationView**的文字顏色可以加入屬性：
``` bash
app:itemTextColor="@color/colorCustomize"
```



## Add an On-click Listener to the Bottom Navigation
監聽**BottomNavigationView**的點擊事件，在代碼中呼叫**setOnNavigationItemSelectedListener()**方法

java:
``` java
bottomNavigationView.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {
        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                
         switch (item.getItemId()) {
             case R.id.main_recents: return true;
             case R.id.main_favorites: return true;
             case R.id.main_nearby: return true;
            }
            return false;
         }
    });
```

運行效果如下：
<img src="bottom_navigation05.gif" width=50% height=50% align=center/>


參考資料：
[Android BottomNavigationView](https://developer.android.com/reference/android/support/design/widget/BottomNavigationView.html)
[Material Design Bottom navigation](https://material.io/guidelines/components/bottom-navigation.html#bottom-navigation-specs)