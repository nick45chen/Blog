---
title: '[MaterialDesign] Android ViewPager'
date: 2018-01-25 18:42:32
tags: ['Android','Material Design']
---

## Introduction

**ViewPager**是在**Android**平台上很常見的組件之一，常用於需要左右滑動的視圖設計上<!-- more -->，接下來我們就直接介紹如何創建一個**ViewPager**

<img src="viewpager01.gif" width=50% height=50% align=center/>

## Create a ViewPager

1.在**Layout**上添加**ViewPager**

layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

   <android.support.v4.view.ViewPager
       android:layout_width="match_parent"
       android:layout_height="220dp"
       android:id="@+id/viewpager_id"/>

</RelativeLayout>
```

2.創建一個**item view**

layout:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.v7.widget.AppCompatTextView
        android:layout_width="match_parent"
        android:layout_height="220dp"
        android:id="@+id/textView_id"
        android:text="1"
        android:gravity="center"
        android:textSize="60sp"
        android:textStyle="bold"/>

</LinearLayout>
```

3.**activity**添加**ViewPager Adapter**

java:
``` java
public class MainActivity extends AppCompatActivity {

    private ViewPager viewPager;
    private ViewPagerAdapter pagerAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        viewPager = findViewById(R.id.viewpager_id);
        
        pagerAdapter = new ViewPagerAdapter(this);
        
        viewPager.setAdapter(pagerAdapter);

    }
}
```

3.**ViewPagerAdapter**是繼承自**PagerAdapter**

java:
``` java
public class ViewPagerAdapter extends PagerAdapter {

    private Context context;
    LayoutInflater inflater;

    public ViewPagerAdapter(Context context) {
        this.context = context;
    }

    public String[] pageNames = {
            "Page 1",
            "Page 2",
            "Page 3",
            "Page 4",
    };


    /**
     * item 數量
     * */
    @Override
    public int getCount() {
        return pageNames.length;
    }

    @Override
    public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
        return (view == object);
    }

    
    /**
     * 添加 item
     * */
    @NonNull
    @Override
    public Object instantiateItem(@NonNull ViewGroup container, int position) {

        inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        // 創建 item view
        View view = inflater.inflate(R.layout.viewpager_item_layout,container,false);
        // 取出 ImageView
        TextView txt = view.findViewById(R.id.textView_id);
        // 綁定視圖資源
        txt.setText(pageNames[position]);
        //添加item view 到 ViewPager
        container.addView(view);

        return view;

    }


    /**
     * 移除 item
     * */
    @Override
    public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
        container.removeView((LinearLayout)object);
    }
}
```

運行效果如下：
<img src="viewpager01.gif" width=50% height=50% align=center/>

參考資料：
[Android Using ViewPager for Screen Slides](https://developer.android.com/training/animation/screen-slide.html)


