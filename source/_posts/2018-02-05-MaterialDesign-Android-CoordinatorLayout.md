---
title: '[MaterialDesign] Android CoordinatorLayout'
date: 2018-02-05 15:56:55
tags: ['Android','Material Design']
---

## Introduction
**CoordinatorLayout**，字面上翻譯過來是一個協調者佈局，官方解釋為它是一個超級**FrameLayout**。<!-- more -->
大部分使用場景中都搭配**AppbarLayout**、**CollapsingToolbarLayout**、**Toolbar**來使用，導致在不知不覺中，大家也有可能認為**CoordinatorLayout**只能跟這幾個佈局一起使用。
其實**CoordinatorLayout**的功能可不止於此，本文就簡單介紹**CoordinatorLayout**的強大功能

**CoordinatorLayout**最主要的兩個作用
1.當作根佈局
2.當作主要容器，協調一個或多個子View行為

接下來我們來看看範例：

<img src="coordinator01.gif" width=50% height=50% align=center/>

可以看到上圖佈局中有兩個**Button1**、**Button2**，移動**Button1**，**Button2**也會跟著移動。

如果不使用**CoordinatorLayout**來實現，那麼兩個**Button**就必須相互持有，然後再**onTouchEvent裡做判斷**，如果需要更多的**View**根據**Button1**的移動，做出相應的響應，實現起來除了耦合度很高，要寫出完整的判斷是也相當不容易。

## Behaviors
**CoordinatorLayout**使用核心就是**Behaviors**，透過**Behaviors**可以執行你訂製的動作，在**Behaviors**中，有兩種角色
1.**Dependency**指的是**Behaviors**依賴的View，如上圖中的**Button1**
2.**Child**指的是要執行**Behaviors**動作的子**View**，如上圖的**Button2**

接下來我們來實作**Behavior**去繼承**CoordinatorLayout.Behavior<T>**，泛行參數T也就是我們要執行動作的**View**類，然後實現兩種方法。
``` java
 @Override
    public boolean layoutDependsOn(CoordinatorLayout parent, T child, View dependency) {
    	// 判斷 child 是否依賴 Dependency
    	// 返回 false 表示 child 不依賴 Dependency
        return super.layoutDependsOn(parent, child, dependency);
    }
 @Override
    public boolean onDependentViewChanged(CoordinatorLayout parent, T child, View dependency) {
    	// child 具體要執行的行為
    	// 當 Dependency 發生變化時(位置、寬、高等)會執行此方法
    	// 返回 true 表示 child 要作出相應的變化，否則 false
        return super.onDependentViewChanged(parent, child, dependency);
    }
```

有了上述的觀念後接下來實作**MyBehavior**
``` java
public class MyBehavior extends CoordinatorLayout.Behavior<Button> {
   
    public MyBehavior(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    public boolean layoutDependsOn(CoordinatorLayout parent, Button child, View dependency) {
    	// 如果依賴是 CustomButton 的實例，返回 true
        return dependency instanceof CustomButton;
    }

    @Override
    public boolean onDependentViewChanged(CoordinatorLayout parent, Button child, View dependency) {

        // 根據 dependency 的位置變化，設置 Button 的位置 

        int top = dependency.getTop();

        setPosition(child, top);
        return true;
    }

    private void setPosition(View view, int y) {
        CoordinatorLayout.MarginLayoutParams layoutParams = (CoordinatorLayout.MarginLayoutParams)view.getLayoutParams();
        layoutParams.topMargin = y;
        view.setLayoutParams(layoutParams);
    }
}
```

在**Layout**裡添加自訂義的**MyBehavior**，**Button**就會依照**Dependency(CustomButton)**的變化，作出相應的行為
``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="net.nickchen45.coordinatorlayout.MainActivity">

    <android.support.v7.widget.AppCompatButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_two"
        android:background="@color/teal_500"
        android:textColor="@android:color/white"
        android:layout_marginTop="0dp"
        android:layout_gravity="right"
        app:layout_behavior="net.nickchen45.coordinatorlayout.MyBehavior"/>

    <net.nickchen45.coordinatorlayout.CustomButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_one"
        android:background="@color/light_blue_500"
        android:textColor="@android:color/white" />


</android.support.design.widget.CoordinatorLayout>

```

<img src="coordinator01.gif" width=50% height=50% align=center/>

就算增加一個**Button3**，也是輕而易舉

<img src="coordinator02.gif" width=50% height=50% align=center/>

參考資料：
[Android CoordinatorLayout](https://developer.android.com/reference/android/support/design/widget/CoordinatorLayout.html)
[Android CoordinatorLayout.Behavior](https://developer.android.com/reference/android/support/design/widget/CoordinatorLayout.Behavior.html)

