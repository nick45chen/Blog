---
title: '[Android] Gaussian blur Effect'
date: 2018-12-01 16:50:50
tags: ['Android']
---
## Introduction
本篇文章來介紹，如何在`Android`上，簡單實現高斯模糊（Gaussian blur)效果。又稱毛玻璃效果或磨砂效果。透過使用`Google`提供的`RenderScript`Api，一個強大的圖片處理框架，幫助`Android`開發者專注於圖片處理算法的邏輯，而不是處理圖像複雜的計算。<!-- more -->

## RenderScript
`RenderScript`根據`Android`官方網站的介紹，在計算的效率上，能充分利用GPU，CPU的計算能力，所以在編寫代碼的時候，毋須擔心具體硬件的不同，都能寫出具高效率的代碼。此篇文章實現的`Gaussian blur`效果，只是使用到了`RenderScript `其中一個操作類`ScriptIntrinsicBlur`，`RenderScript`Api的強大遠不及此，更詳情可以參考 [Android 官網 RenderScript介紹](https://developer.android.com/guide/topics/renderscript/compute)

### 添加Api依賴庫
添加此依賴庫也很簡單，只要在`app/build.gradle`文件，在`android/defaultConfig`配置中添加兩行代碼：

``` xml
android {
	...
   defaultConfig {
    ...
      renderscriptTargetApi 18
      renderscriptSupportModeEnabled true
   }
}
```

代碼實現起來也很簡單，如下：

``` java
public Bitmap blurBitmap(Bitmap bitmap) {

        //Let's create an empty bitmap with the same size of the bitmap we want to blur
        Bitmap outBitmap = Bitmap.createBitmap(bitmap.getWidth(), bitmap.getHeight(), Bitmap.Config.ARGB_8888);

        //Instantiate a new Renderscript
        RenderScript rs = RenderScript.create(getApplicationContext());

        //Create an Intrinsic Blur Script using the Renderscript
        ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.create(rs, Element.U8_4(rs));

        //Create the Allocations (in/out) with the Renderscript and the in/out bitmaps
        Allocation allIn = Allocation.createFromBitmap(rs, bitmap);
        Allocation allOut = Allocation.createFromBitmap(rs, outBitmap);

        //Set the radius of the blur: 0 < radius <= 25
        blurScript.setRadius(25.0f);

        //Perform the Renderscript
        blurScript.setInput(allIn);
        blurScript.forEach(allOut);

        //Copy the final bitmap created by the out Allocation to the outBitmap
        allOut.copyTo(outBitmap);

        //recycle the original bitmap
        bitmap.recycle();

        //After finishing everything, we destroy the Renderscript.
        rs.destroy();

        return outBitmap;

    }
```
透過設置模糊半徑`radius `大小來控制圖片的清晰度。
</br>
這邊值得注意的是`ScriptIntrinsicBlur`只支援API17以上版本，為了兼容舊版，Google提供了`android.support.v8.renderscript`兼容包，使用時只要引入該兼容包下相關的類別即可。
</br>
</br>
效果如下:
</br>
</br>
<img src="gaussian_blur_effect01.gif" width=50% height=50% align=center/>


</br>
</br>
</br>
</br>
</br>
</br>
</br>

參考資料：
</br>
[Android RenderScript](https://developer.android.com/guide/topics/renderscript/compute)
</br>
[維基百科介紹 Renderscript](https://zh.wikipedia.org/wiki/Renderscript)
</br>
[CSDN-huachao1001部落格](https://blog.csdn.net/huachao1001/article/details/51524502)
</br>
