---
title: '[Material Design] Android Toolbar(一)'
date: 2017-11-19 21:13:47
tags: ['Android','Material Design']
---

## Introduction

Toolbar 是 Google 在Android 5.0 推出的一個 Material Design 的控件，Toolbar 的靈活性在於並不只限於只能放在上方導航欄的位置上，同時也能嵌套在任意的 View 結構中，當然你可以在 Activity 中，呼叫  setActionBar() 方法，把 Toolbar 指定為上方導航欄使用。
<!-- more --> 
Toolbar 還開放讓開發者，自訂：

1.支持自訂按鈕
2.支持設置品牌Logo
3.支持設置標題和子標題
4.支持添加一個或多個自定義的控件
5.支持設置 Action Menu

## Create Toolbar

1.進入 res -> values -> style ，修改App的樣式為 NoActionBar

``` bash
<resources>
	...
	 <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
	        <item name="colorPrimary">@color/colorPrimary</item>
	        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
	        <item name="colorAccent">@color/colorAccent</item>
	 </style>
</resources>
```

2.在 Activity 的 Layout XML 引用 support.v7 包下的 Toolbar

``` bash
<?xml version="1.0" encoding="utf-8"?>
 <RelativeLayout
 	... >
 	...
	 <android.support.v7.widget.Toolbar
	 	.. />

</RelativeLayout>
```

3.如果我們讓 Toolbar 同系統預設的 Action bar 高，寬就使用 match_parent 符合螢幕就好，顏色也是同系統預設 ActionBar 的顏色

``` bash
<android.support.v7.widget.Toolbar
	android:id="@+id/toolbar"
	android:layout_width="match_parent"
	android:layout_height="?attr/actionBarSize"
	android:background="?attr/colorPrimary"/>
```

4.XML 完成後，我們回到 Activity ，呼叫 Activity 中的 setActionBar() 方法，把 Action bar 換成自己的 Toolbar 

``` bash
public class MainActivity extends AppCompatActivity {
	...

	@Override
	protected void onCreate(Bundle saveInstanceState) {
		super.onCreate(saveInstanceState)
		setContentView(R.layout.activity_main);
		...
		setSupportActionBar(toolbar);
	}

}
```

## Customize the Toolbar

接下來我們來客製化自己的 ToolBar 顏色，顏色的來源就從 Google Material 官網選擇
[Material Design Color](https://material.io/guidelines/style/color.html#color-color-palette)
或是參考熱心網友整理成的XML
[Android Material Design Colours](https://gist.github.com/daniellevass/b0b8cfa773488e138037)

1.進入res -> values -> colors ，改變 color primary 的顏色，就可以換掉我們的 Toolbar 顏色了，通常我們會連 color promary dark 一起換掉。
(1) color primary 代表 Toolbar 顏色，通常我們會選擇數值 400 ~ 600間
(2) color primary 代表 Status Bar (Action Bar 上面顯示電池電量那一欄) 顏色，通常我們會選擇顏色深一點 數值約 700 ~ 900間
我們是選擇 Red-500/700
``` bash
<resources>
	...
	<item name="colorPrimary">#F44336</item>
	<item name="colorPrimaryDark">#D32F2F</item>
	<item name="colorAccent">#D32F2F</item>
</resources>
```

完成到了這邊可以看到預覽效果
<img src="toolbar01.png" width=50% height=50% align=center/>

這邊你會想說，ToolBar Label 怎麼是黑色的，我想要換成白色該怎麼辦?!
別擔心，只要把 ToolBar Theme 換成 Dark ActionBar就好，此 Theme 的文字顏色是白色

``` bash
 <android.support.v7.widget.Toolbar
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

顯示出來的效果如下：
<img src="toolbar02.png" width=50% height=50% align=center/>

或許眼尖的你會發現，有些手機在螢幕下方有一排 Navigation Bar，如果我們有需求要改變顏色怎麼辦!?
只要在 res -> values -> style 找到 AppTheme 中更改顏色就好，我們也把它設定成跟 Status Bar 顏色相同

``` bash
<resources>
	...
	<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
		<item name="colorPrimary">@color/colorPrimary</item>
		<item name="colorPrimaryDark">@color/colorPrimaryDark</item>
		<item name="colorAccent">@color/colorAccent</item>
		<item name="android:navigationBarColor">@color/colorPrimaryDark</item>
	 </style>
</resources>
```

顯示出來的效果如下：
<img src="toolbar03.png" width=50% height=50% align=center/>

參考網站：
[Android ToolBar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)
[Material Design](https://material.io/guidelines/)




