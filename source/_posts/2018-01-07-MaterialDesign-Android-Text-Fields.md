---
title: '[Material Design] Android Text Fields'
date: 2018-01-07 16:17:53
tags: ['Android','Material Design']
---

## Introduction

Text Fields 文本框，是一種允許使用者輸入、編輯、選擇文字的封裝類，常出現在表單中，不過也可以用在 Dialog 或是 搜尋框中。<!-- more -->與 TextInputLayout 搭配使用，可以讓你的使用者體驗更佳

在本篇文章中，我們要使用 support.v7 library 下的 **AppCompatEditText** 與 **TextInputLayout** 來實現我們下面的數種效果：

## Add a Text Input Layout to the AppCompat EditText

### Add Design Library To Our Project

首先我們先打開 File -> Project structure -> app -> Dependencies，把 design library 添加進來
 <img src="textFields05.png" width=100% height=100% align=center/>

## Add Text Input Layout to our Layout 
創建一個 TextInputLayout，並把 AppCompatEditText 放到裡面就完成了，運行起來會看到 hint 的文字透過動畫，上移到輸入匡的左上方

**xml:**
``` bash
<android.support.design.widget.TextInputLayout
        android:id="@+id/username_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/username_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/username"
            android:inputType="text" />

    </android.support.design.widget.TextInputLayout>
```

運行效果如下：
 <img src="textFields06.gif" width=50% height=50% align=center/>

這邊大家可以發現，focus 預設是在第一個 **AppCompatEditText** 上，如果想改變 focus，只要在第二個 **AppCompatEditText** tag 結尾加上，**< requestFocus />** tag 就可以了

**xml:**
``` bash
<android.support.design.widget.TextInputLayout
        android:id="@+id/password_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/username_text_input_layout"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/password_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/password"
            android:inputType="textPassword" /><requestFocus/>

    </android.support.design.widget.TextInputLayout>
```

運行效果如下：
 <img src="textFields12.png" width=50% height=50% align=center/>


如果我們不想一開始就 focus 在任一個 **AppCompatEditText** 上，我們可以設定預設 focus 在最外層的 **ViewGroup** 上。
在最外層的 **ViewGroup** 上，添加兩個屬性 : 
- android:focusable="true"
- android:focusableInTouchMode="true"

**xml:**
``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/activity_main"
    android:focusable="true"
    android:focusableInTouchMode="true">

    ...

</RelativeLayout>
```

運行效果如下：
<img src="textFields08.png" width=50% height=50% align=center/>

如果我們還想要，在點擊 **AppCompatEditText** 以外的地方，移除 **AppCompatEditText** 的 focus 怎麼辦呢？最直接的解決辦法就是在最外層的 **ViewGroup** 設定點擊事件就好了，這樣當用戶點擊 **AppCompatEditText** 以外的地方(其實就是點擊 **ViewGroup** )，focus 就會移轉到**ViewGroup** 上。  

回到我們的代碼中，新增 **ViewGroup** 點擊事件

**java:**
``` java
public class MainActivity extends AppCompatActivity {

    RelativeLayout relativeLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        relativeLayout = findViewById(R.id.activity_main);
        relativeLayout.setOnClickListener(null);// 設定點擊事件
    }

    ...

}

```
運行效果如下：
 <img src="textFields09.gif" width=50% height=50% align=center/>

## More features of AppCompat EditText

**TextInputLayout** 還提供一些方便的方法

### 顯示錯誤訊息
可以清楚明暸的回饋給使用者一些錯誤的訊息，比如：輸入必填欄位

**xml:**
``` bash
<android.support.design.widget.TextInputLayout
        android:id="@+id/username_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/username_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/username"
            android:inputType="text" />

    </android.support.design.widget.TextInputLayout>
```

**java:**
``` java
userTextInputLayout.setErrorEnabled(true);
userTextInputLayout.setError("please input user name");
```

運行效果如下：
<img src="textFields10.png" width=50% height=50% align=center/>

### 密碼明碼顯示
很常見的功能，可以讓使用者在送出密碼前，自己檢查密碼正確性，只要在 **TextInputLayout** tag 裡添加，**app:passwordToggleEnabled="true"**，即可啟用

**xml:**
``` bash
<android.support.design.widget.TextInputLayout
        android:id="@+id/password_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/username_text_input_layout"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp"
        app:passwordToggleEnabled="true">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/password_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/password"
            android:inputType="textPassword" />

    </android.support.design.widget.TextInputLayout>
```
運行效果如下：
<img src="textFields11.png" width=50% height=50% align=center/>

### 字元統計
提供方便的字數提示，只需要在 **TextInputLayout** tag 中，添加屬性 **app:counterEnabled** 及 **app:counterMaxLength** 就可以了

**xml:**
``` bash
 <android.support.design.widget.TextInputLayout
        android:id="@+id/password_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/username_text_input_layout"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp"
        app:counterEnabled="true"
        app:counterMaxLength="10">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/password_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/password"
            android:inputType="textPassword" /><requestFocus/>

    </android.support.design.widget.TextInputLayout>
```

也可以以代碼的方式啟用

**java:**
``` java
passwordTextInputLayout.setCounterEnabled(true);
passwordTextInputLayout.setCounterMaxLength(10);
```
運行效果如下：
<img src="textFields13.png" width=50% height=50% align=center/>
超過字數時
<img src="textFields14.png" width=50% height=50% align=center/>

### 改變顏色
系統預設正常輸入下是稍微偏紅色，與字元計數器超過自定義限制時，顏色是紫色，我們可以透過宣告 style 的方式來改變外觀

**style:**
``` bash
<style name="textInputLayoutStyle" parent="AppTheme">
    <item name="colorAccent">@color/colorTextInputNormal</item>
</style>

<style name="characterOverFlowStyle">
    <item name="android:textColor">@color/colorTextInputError</item>
</style>
```

接著將 style 設定到 **TextInputLayout** 的屬性裡

**xml:**
``` bash
 <android.support.design.widget.TextInputLayout
        android:id="@+id/password_text_input_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/username_text_input_layout"
        android:layout_marginBottom="8dp"
        android:layout_marginLeft="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="32dp"
        android:theme="@style/textInputLayoutStyle"
        app:counterEnabled="true"
        app:counterMaxLength="10"
        app:counterOverflowTextAppearance="@style/characterOverFlowStyle">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/password_text_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/password"
            android:inputType="textPassword" />

    </android.support.design.widget.TextInputLayout>
```

運行效果如下：
正常狀態下，顯示 colorTextInputNormal 顏色
<img src="textFields15.png" width=50% height=50% align=center/>
超過字數時，顯示 colorTextInputError 顏色
<img src="textFields16.png" width=50% height=50% align=center/>

- 改變**hint**的顏色也可以透過 **app:hintTextAppearance** 方法
- 改變錯誤訊息的外觀顏色 **app:errorTextAppearance** 方法
設定方法就同設定 **app:counterOverflowTextAppearance** 與 **characterOverFlowStyle**


參考網站:
[Android Developer](https://developer.android.com/reference/android/support/design/widget/TextInputLayout.html)
[Material Design](https://material.io/guidelines/components/text-fields.html#)


