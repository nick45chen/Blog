---
title: '[MaterialDesign] Android Collapsing Toolbar Layout'
date: 2018-02-21 21:49:56
tags: ['Android','Material Design']
---

## Introduction
**CollapsingToolbarLayout**是被設計為**AppBarLayout**的子類，用於完成一些折疊的UI視覺效果。<!-- more -->**CollapsingToolbarLayout**的功能大概有以下幾點：

### Collapsing title
折疊的Title文字效果，顧名思義在`AppBar`，完全展開時，會顯示比較大的字型，不過當滑動在螢幕外時，字體會在摺疊動畫結束後，顯示比較小的字型。顯示的文字可以透過屬性`setTitle(CharSequence)`設置，文字的樣式，可以透過`collapsedTextAppearance`與`expandedTextAppearance`兩個屬性調整。

### Content scrim
當`Appbar`滑動到一定的程度時，所顯示/隱藏的主體顏色，即`Toolbar`顏色。可以透過`setContentScrim(Drawable)`

### Status bar scrim
跟**Content scrim**相同，不過是狀態列的顏色，可以透過屬性**setStatusBarScrim(Drawable)**設置，不過只有在Android系統版本5.0以上，並且設定**fit system windows**時有效果

### Parallax scrolling children
子類別可以在滾動時選擇，是否以"視差"的方式跟隨滾動，可以透過屬性**layout_collapseMode="parallax"**設定

## Add the Design Library
在使用**CollapsingToolbarLayout**前，必須先將**Design Library**加入專案中。在File -> Project Structure -> Dependencies 下新增
<img src="collapsing_toolbar01.png" width=100% height=100% align=center/>

## Create a Collapsing Toolbar Layout

### Change the Default App Theme
接下來我們必須創建自己的**Toolbar**，首先將**style.xml**裡的**AppTheme**改為**NoActionBar**，此舉將讓每頁**Activity**預設無**Action Bar**
style.xml:
``` bash
<resources>
	...
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
	...
</resources>
```
### Create a Toolbar
接下來在**Activity.xml**裡，新增一個**Toolbar**，前面我們有介紹過，**CollapsingToolbarLayout**是被設計用來實現**AppBarLayout**折疊UI效果的子類，所以我們必須在**AppBarLayout**下創建它，並在**CollapsingToolbarLayout**裡新增一張圖片，來作為**AppBarLayout**展開時的背景。

如果還不清楚[CoordinatorLayout](http://nickcode4fun.net/2018/02/05/MaterialDesign-Android-CoordinatorLayout/)與[AppBarLayout](http://nickcode4fun.net/2018/02/07/MaterialDesign-Android-AppBarLayout/)，可以點連結查看

layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:fitsSystemWindows="true"
    tools:context="net.nickcode4fun.collapsingtoolbarlayout.MainActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/app_bar_layout"
        android:fitsSystemWindows="true"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/collapsingToolbarLayout"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">
            
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="300dp"
                android:fitsSystemWindows="true"
                android:src="@drawable/bg_appbar"
                android:scaleType="fitXY"
                app:layout_collapseMode="parallax"
                android:contentDescription="@string/firework" />

            <android.support.v7.widget.Toolbar
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:id="@+id/toolbar"
                app:layout_collapseMode="parallax" />

        </android.support.design.widget.CollapsingToolbarLayout>


    </android.support.design.widget.AppBarLayout>

</android.support.design.widget.CoordinatorLayout>
```

並在`Activity.java`中，設置`Toolbar`

``` java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setSupportActionBar((Toolbar) findViewById(R.id.toolbar));
    }
}
```

運行效果如下：
<img src="collapsing_toolbar02.gif" width=50% height=50% align=center/>

</br>
</br>
</br>
</br>
</br>

參考資料：
[Android CollapsingToolbarLayout](https://developer.android.com/reference/android/support/design/widget/CollapsingToolbarLayout.html)