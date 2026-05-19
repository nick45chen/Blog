---
title: '[Material Design] Android Toolbar(二) - Pop-up Menu'
date: 2017-11-24 13:27:42
tags: ['Android','Material Design']
---

## Introduction

接下來我們來介紹 Pop-up Menu ， Pop-up Menu 使用在很多應用中，對開發者跟使用者都非常方便，而且我們可以自定義功能項，加到我們的 Toolbar 中。
<!-- more -->
<img src="popup01.png" width=50% height=50% align=center/>

## Create Pop-up Menu

1.在res目錄下，創建 menu file
<img src="popup02.png" width=50% height=50% align=center/>

2.進入剛剛創建的 menu file 新增 item tag，然後再 tag 中定義屬性 id 與 title，title 為顯示在 UI 畫面上的名稱
<img src="popup03.png" width=50% height=50% align=center/>

3.回到 Activity 中，Override 兩個 Methods

``` bash
public class MainActivity extends AppCompatActivity {
	...

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		return super.onCreateOptionsMenu(menu);
	}

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		return super.onOptionsItemSelected(item);
	}

}
```

4.使用 onCreateOptionsMenu() 創建 Pop-up Menu ，並使用 onOptionsItemSelected() 監聽點擊事件，點擊事件我們用 id 判別不同的 item 項

``` bash
	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
    	getMenuInflater().inflate(R.menu.menu_main, menu);
    	return true;
    }

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
    	//取得 menu id 來判斷是哪一個 item 被選中，並用 Toast 回饋給使用者
    	switch (item.getItemId()) {
            case R.id.item1_id:
                Toast.makeText(this, "item 1", Toast.LENGTH_SHORT).show();
                break;
            case R.id.item2_id:
                Toast.makeText(this, "item 2", Toast.LENGTH_SHORT).show();
                break;
            case R.id.item3_id:
                Toast.makeText(this, "item 3", Toast.LENGTH_SHORT).show();
                break;
        }
        return super.onOptionsItemSelected(item);
    }
```

5.之後就可以在畫面上我們創建的 Menu，點擊 item 1 會觸發點擊事件
<img src="popup04.png" width=50% height=50% align=center/>
<img src="popup05.png" width=50% height=50% align=center/>

## Customize Pop-up Menu

在預設的 Pop-up Menu 中，背景顏色是黑色，對於我們的 App theme 看來非常不美觀，所以我們要做的第一件事是把 Pop-up Menu 的背景顏色換掉

### Change Background Color

1.首先進入 res -> values -> style 目錄下，新增一個 style tag ，名稱我們叫做 PopupTheme 並新增一個屬性 android:background ，這邊你可以自定義 Pop-up Menu 的背景顏色，我們使用白色

``` bash
<resources>
	...
	<style name="PopupTheme" parent="Theme.AppCompat.Light">
		<item name="android:background">@android:color/white</item>
	</style>
</resources>
```

2.接著把自定義的 PopupTheme 替換掉在 AppTheme 裡 popupTheme 的屬性

``` bash
<resources>
	<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
		<item name="colorPrimary">@color/colorPrimary</item>
		<item name="colorPrimaryDark">@color/colorPrimaryDark</item>
		<item name="colorAccent">@color/colorAccent</item>
		<item name="popupTheme">@style/PopupTheme</item>
	</style>

	<style name="PopupTheme" parent="Theme.AppCompat.Light">
		<item name="android:background">@android:color/white</item>
	</style>
	...
</resources>
```

接著就可以看到效果
<img src="popup06.png" width=50% height=50% align=center/>

### Change Text Color

如果你還有改變文字顏色的需求，你可以在剛剛自定義的 PopupTheme 中，添加屬性 android:textColor 自定義文字顏色

``` bash
<resources>
	<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
		<item name="colorPrimary">@color/colorPrimary</item>
		<item name="colorPrimaryDark">@color/colorPrimaryDark</item>
		<item name="colorAccent">@color/colorAccent</item>
		<item name="popupTheme">@style/PopupTheme</item>
	</style>

	<style name="PopupTheme" parent="Theme.AppCompat.Light">
		<item name="android:background">@android:color/white</item>
		<item name="android:textColor">@color/colorPrimary</item>
	</style>
	...
</resources>
```

效果如下
<img src="popup07.png" width=50% height=50% align=center/>

參考網站：
[Android ToolBar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)
[Material Design](https://material.io/guidelines/)






