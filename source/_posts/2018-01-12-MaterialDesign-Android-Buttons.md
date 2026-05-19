---
title: '[MaterialDesign] Android Buttons'
date: 2018-01-12 14:43:24
tags: ['Android','Material Design']
---

## Introduction

在**Material Design**中，Button 共分為三類<!-- more -->

### Raised button
凸起式按鈕，按下去會有一個下壓的效果
<img src="button01.png" width=50% height=50% align=center/>


### Flat button
平面按鈕，也稱作純文本按鈕，並沒有像**Raised buttons**一樣，有下壓效果，常用於 **dialog**、**toolbar**中
<img src="button02.png" width=50% height=50% align=center/>
<img src="button03.png" width=50% height=50% align=center/>

### Floating button
顧名思義就是浮著的Button，在設計上通常一個介面只會存在一個，代表此頁最常見的功能鈕
<img src="button04.png" width=50% height=50% align=center/>
<img src="button05.png" width=50% height=50% align=center/>

接下來我們來看看在**Android**裡如何實現。

## Create a Raised button
使用**Raised buttons**很容易，只要在 layout 裡直接引用**AppCompat Library**裡的，具有**Material features** 的 **AppCompatButton** 就可以了

**layout:**
``` bash
  <android.support.v7.widget.AppCompatButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/raised_button"/>
```

運行效果如下：
<img src="button06.png" width=50% height=50% align=center/>

如何更換**AppCompatButton**的按鈕顏色呢？
首先打開**styles.xml**文件，在裡面創建一個 new style 定義背景及文字顏色

**styles.xml:**
``` bash
<resources>
   ...
   <style name="RaiseButton" parent="Widget.AppCompat.Button.Colored">
        <item name="android:backgroundTint">@color/colorRaisedButton</item>
        <item name="android:textColor">@color/colorRaisedButtonText</item>
   </style>
</resources>
```

接著再回到 layout 文件的**AppCompatButton**中，設定剛剛新增的 style

**layout:**
``` bash
 <android.support.v7.widget.AppCompatButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/raised_button"
        style="@style/RaiseButton"/>
```

運行效果如下：
<img src="button07.png" width=50% height=50% align=center/>

## Create a Flat button
創建**Flat button**也很容易，打開**styles.xml**文件，在裡面創建一個 new style，**Widget.AppCompat.Button.Borderless** 表示你的按鈕背景為透明，當然你也可以在**style**裡，定義文字的顏色

**styles.xml:**
``` bash
<resources>
   ...
   <style name="FlatButton" parent="Widget.AppCompat.Button.Borderless">
     
   </style>
</resources>
```

接著創建一個**AppCompatButton**，把**style**設定進去就好

**layout:**
``` bash
<android.support.v7.widget.AppCompatButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/flat_button"
        style="@style/FlatButton"/>
```

運行效果如下：
<img src="button08.png" width=50% height=50% align=center/>

## Create a Floating Action Button
接下來我們來創建**Floating Action Button**，首先要先確認**Design Library**有加進我們的**Project**
<img src="button09.png" width=100% height=100% align=center/>

之後就可以在 layout 裡直接引用了

**layout:**
``` bash
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

運行效果如下：
<img src="button10.png" width=50% height=50% align=center/>

你也可以在**Floating Action Button**中，添加icon

**layout:**
``` bash
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_dialog_email" />
```

運行效果如下：
<img src="button11.png" width=50% height=50% align=center/>

設定**Floating Action Button**的size
以 size 可分為:
**1. normal circle = 56x56dp**
**2.Mini circle = 40x40dp**

**layout:**
``` bash
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_dialog_email"
        app:fabSize="normal" />
```

運行效果如下：
<img src="button12.png" width=70% height=70% align=center/>

**Floating Action Button**還有一個很符合此命名的屬性 elevation
其中 elevation 設定的 DP 越大，則按鈕背後的陰影也會越大

**layout:**
``` bash
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_dialog_email"
        app:fabSize="normal"
        app:elevation="8dp" />
```

運行效果如下：
<img src="button13.png" width=70% height=70% align=center/>

**Floating Action Button**的背景顏色預設是**@color/colorAccent**，如果想要自訂背景顏色，只要改變屬性:**backgroundTint**就可以了
**layout:**
``` bash
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_dialog_email"
        app:fabSize="normal"
        app:backgroundTint="@color/holo_green_dark" />
```

運行效果如下：
<img src="button14.png" width=50% height=50% align=center/>



參考網站:
[Android Developer](https://developer.android.com/reference/android/widget/Button.html)
[Material Design Buttons](https://material.io/guidelines/components/buttons.html#)
[Material Design Buttons: Floating Action Button](https://material.io/guidelines/components/buttons-floating-action-button.html#buttons-floating-action-button-floating-action-button)