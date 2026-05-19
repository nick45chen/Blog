---
title: '[MaterialDesign] Android Snackbar'
date: 2018-01-22 10:30:45
tags: ['Android','Material Design']
---

## Introduction

在**Android**開發中，每次當想要像用戶顯示一些訊息，我們可能會選擇**Toast**，但**Toast**使用起來卻有一些限制，不能在文本訊息顯示的同時，包含一些文本的操作，**Google**在**support:design library**下，推出了**Snackbar**可以讓使用者在文本訊息的使用上更美觀，同時擁有更多的彈性<!-- more -->

**Snackbar**與**Toast**同時都為顯示在螢幕底部的文本訊息，**Snackbar**擁有以下特性：
1.只能顯示文字，無法添加**icon**
2.同時只能顯示單一個**Snackbar**，需連續顯示時，會等上一個**dismiss**後，才顯示
3.只能定義一個**action**，且功能不是取消，如果需要兩個以上的**action**，請使用**Dialogs**

接下來我們就來實作**Snackbar**

## Create a Snackbar
要使用**Snackbar**，首先需要把**support:design library**添加進我們的**Project**中，在 File -> Project Structure -> Dependencies 下新增。
<img src="snackbar01.png" width=100% height=100% align=center/>

接下來在代碼中使用**Snackbar**，第一個參數**view**必須傳入對應的**根佈局**，第二的參數是顯示文字，第三個參數是顯示的時間。
``` java
Snackbar.make(@NonNull View view, @NonNull CharSequence text, @Duration int duration).show();
```
``` java
View rootView = findViewById(R.id.activity_main);
Snackbar.make(rootView, "Archived", Snackbar.LENGTH_LONG).show();
```
也可以透過代碼指定**Snackbar**持續的時間
``` java
snackbar = Snackbar.make(root, "Archived", Snackbar.LENGTH_INDEFINITE);
snackbar.setDuration(3000); // Snackbar.LENGTH_INDEFINITE 如果沒有指定時間，Snackbar將需不會取消
snackbar.show();
```

運行效果如下：

<img src="snackbar02.gif" width=50% height=50% align=center/>

## Create a Snackbar Using CoordinatorLayout

如果同時使用 **FloatingActionButton** 與 **Snackbar**，會發現 **Snackbar** 高度會覆蓋 **FloatingActionButton**，如果要解決這個問題，就要使用 **CoordinatorLayout**

**activity**佈局如下，把**FloatingActionButton**放進**CoordinatorLayout**tag裡

layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/activity_main">

    <android.support.design.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <android.support.design.widget.FloatingActionButton
            android:id="@+id/fab_btn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:src="@drawable/ic_add_white_24dp"
            app:fabSize="normal" />

    </android.support.design.widget.CoordinatorLayout>


</android.support.constraint.ConstraintLayout>
```

java:
``` java
 floatingActionButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Archived", Snackbar.LENGTH_SHORT).show();
            }
        });
```

運行效果如下：
<img src="snackbar03.gif" width=50% height=50% align=center/>

搭配**CoordinatorLayout**，可以使**Snackbar**出現在任何**ViewGroup的底部**，做出下方效果
<img src="snackbar04.gif" width=50% height=50% align=center/>

## Add an Action to the Snackbar
如果要添加文字訊息對應的 **action** 操作的話，可以使用 **setAction()** 方法
``` java
snackbar.setAction(CharSequence text, final View.OnClickListener listener);
```

java:
``` java
snackbar = Snackbar.make(rootView, "Archived", Snackbar.LENGTH_INDEFINITE);
snackbar.setDuration(5000);
snackbar.setAction("UNDO", new View.OnClickListener() {
    @Override
    public void onClick(View v) {
		// 點擊事件
     }
});
snackbar.show();
```

運行效果如下：
<img src="snackbar05.gif" width=50% height=50% align=center/>

## Customize the Snackbar
如要改變**Snackbar**的含背景、訊息文字、按鈕文字顏色，則需要在呼叫**show()**方法前，增加以下代碼

1.背景顏色

java:
``` java
// 取出snackbar view
View view = snackbar.getView();
// 更換view背景顏色
view.setBackgroundColor(ContextCompat.getColor(MainActivity.this, R.color.colorSnackbarBackground));
```

運行效果如下：
<img src="snackbar06.png" width=50% height=50% align=center/>

2.訊息文字顏色

java:
``` java
// 取出 message textview
TextView textView = snackbar.getView().findViewById(android.support.design.R.id.snackbar_text);
// 改變文字顏色
textView.setTextColor(ContextCompat.getColor(MainActivity.this, R.color.colorSnackbarText));
```

運行效果如下：
<img src="snackbar07.png" width=50% height=50% align=center/>

3.**Action**文字顏色

java:
``` java
// 取出 action textview
TextView textView = snackbar.getView().findViewById(android.support.design.R.id.snackbar_action);
// 改變文字顏色
textView.setTextColor(ContextCompat.getColor(MainActivity.this, R.color.colorSnackbarAction));
```

運行效果如下：
<img src="snackbar08.png" width=50% height=50% align=center/>


參考網站:
[Android Snackbar](https://developer.android.com/reference/android/support/design/widget/Snackbar.html)
[Material Design Snackbars & toasts](https://material.io/guidelines/components/snackbars-toasts.html)






