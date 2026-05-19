---
title: '[MaterialDesign] Android AppBarLayout'
date: 2018-02-07 11:17:04
tags: ['Android','Material Design']
---

## Introduction
**AppBarLayout**顧名思義，就是設計在**導航欄(Toolbar)**、**頂部頁籤欄(TabLayout)**一起使用，來達成一些**Material Design**風格的一些滑動交互效果。<!-- more -->
<img src="appbar01.gif" width=50% height=50% align=center/>

**AppBarLayout**本質上是一個垂直的**LinearLayout**，不過為了實現滑動交互的效果，因此嚴重依賴於**CoordinatorLayout**，如果在別的**ViewGroup**裡，則無法發揮**AppBarLayout**大部分的功能。

**AppBarLayout**同時要求**Child**明確的設置各自的**AppBarLayout.ScrollingViewBehavior**，在代碼中可以使用**setScrollFlags(int)**方法，在佈局中可以使用**app:layout_scrollFlags**屬性。

設定**scrollFlags**，必須使用**AppBarLayout.LayoutParams**中已經定義好的五個常量：
**SCROLL_FLAG_SCROLL**
**SCROLL_FLAG_ENTER_ALWAYS**
**SCROLL_FLAG_ENTER_ALWAYS_COLLAPSED**
**SCROLL_FLAG_EXIT_UNTIL_COLLAPSED**
**SCROLL_FLAG_SNAP**

接下來我們將一一介紹這些**flag**的不同

## scroll
**Child View**跟滾動事件有直接的關係，也就是說只要手指一滑動，**AppBar**必定會隨著滑動往上推，或滑動到頂部顯示，這邊有兩點要注意一下。

第一點:如果設置其他的**Flag**，必須伴隨著使用**SCROLL_FLAG_SCROLL**才有作用。
第二點:在**AppBarLayout**裡這個**Child View**，前面沒有任何**Child View**設置這個值，那麼這個**Child View**設置將失去任何作用

第一點稍後做說明，第二點的意思如下圖所示:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="net.nickchen45.appbardemo.MainActivity">
    
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/appbar_id">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/child_view"
            android:textSize="22sp"
            android:textColor="@android:color/white"
            android:gravity="center"
            app:layout_scrollFlags="scroll"/>

        <android.support.v7.widget.Toolbar
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:id="@+id/toolbar_id"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" />

    </android.support.design.widget.AppBarLayout>

    ...

</android.support.design.widget.CoordinatorLayout>
```

大家可以看到**AppBarLayout**裡，有兩個**Child View**，第二個**Child View(Toolbar)**，前面有一個**Child View(TextView)**，且設置著**app:layout_scrollFlags="scroll"**。
運行效果如下:
<img src="appbar02.gif" width=50% height=50% align=center/>


如果把第一個**Child View(TextView)**的**Flag移除**，並在第二個**Child View(Toolbar)**中設置，由於第一個**Child View**沒有設置任何**Flag**，所以第二個**Child View(Toolbar)**即使設置**Flag**也沒有效果
``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="net.nickchen45.appbardemo.MainActivity">
    
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/appbar_id">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/child_view"
            android:textSize="22sp"
            android:textColor="@android:color/white"
            android:gravity="center"/>

        <android.support.v7.widget.Toolbar
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:id="@+id/toolbar_id"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:layout_scrollFlags="scroll"/>

    </android.support.design.widget.AppBarLayout>

    ...

</android.support.design.widget.CoordinatorLayout>
```
運行效果如下:
<img src="appbar03.gif" width=50% height=50% align=center/>

## enterAlways
在**app:layout_scrollFlags**設置此屬性，與前者**scroll**對比，前者是先滾動 Scrolling View，後者是先滾動 Child View，當優先滾動的一方全部顯示後，另一方才會開始滾動

在**AppBarLayout**的**Child View**中設置屬性，前面有說明，除了**scroll**之外其他的**Flag**，都需搭配**scroll**使用
``` bash
app:layout_scrollFlags="scroll|enterAlways"
```
運行效果如下:
<img src="appbar04.gif" width=50% height=50% align=center/>

## enterAlwaysCollapsed
**enterAlways**的附加**flag**，向下滾動時，先會顯示**Child View**的最小高度，等待**ScrollView**滑至頂部時，**Child View**再向下滾動，直到完全顯示

需要定義高度、最小高度、**Flag**等屬性
``` bash
android:layout_height="200dp"
android:minHeight="56dp"
app:layout_scrollFlags="scroll|enterAlways|enterAlwaysCollapsed"
```
運行效果如下:
<img src="appbar05.gif" width=50% height=50% align=center/>

## exitUntilCollapsed
當發生向上滾動事件時，**Child View**會退出螢幕外至保留最小高度，向下滑動到頂部時，**Child View**才會向下滾動，直到完全顯示

``` bash
android:layout_height="200dp"
android:minHeight="56dp"
app:layout_scrollFlags="scroll|exitUntilCollapsed"
```
運行效果如下:
<img src="appbar06.gif" width=50% height=50% align=center/>

## snap
**snap**簡單理解，就是**child view**有一個滾動的吸附效果，當**child view**滾動達到一定比例，即使手指放開，也會自動滾動到全部顯示，反之則會退出螢幕

``` bash
android:layout_height="200dp"
app:layout_scrollFlags="scroll|snap"
```
運行效果如下:
<img src="appbar07.gif" width=50% height=50% align=center/>


參考資料：
[AppBarLayout](https://developer.android.com/reference/android/support/design/widget/AppBarLayout.html)
[AppBarLayout.LayoutParams](https://developer.android.com/reference/android/support/design/widget/AppBarLayout.LayoutParams.html#SCROLL_FLAG_SNAP)

