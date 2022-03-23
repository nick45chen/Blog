---
title: '物件導向設計原則(SOLID)#2 - Open Closed Principle'
date: 2020-02-19 14:06:36
tags:
description: 
---

### Open Closed Principle (OCP)

>藉由增加新的程式碼來擴充系統的功能，而不是藉由修改原本已經存在的程式碼來擴充系統的功能。

在軟體開發中，對於擴展是開放，對於修改是封閉，用來避免改A壞B的情形，講白了就是不去動舊的程式碼，而是對於新增的程式碼補上測試就好。

什麼意思呢？
<!-- more -->
<div align="center">
    <img src="example.jpeg" width=30% height=30% align=center>
</div>

#### Example

假如現在要開發第三平台登入功能，以前程式碼可能寫成這樣。

``` java
public class AuthController {
    public void login(Auth authData) {
        if (authData.type.equals("facebook")) {
            // 實作 facebook 登入
        } else if (authData.type.equals("google")){
            // 實作 google 登入
        } else {
            throw new IllegalStateException();
        }
    }
}
```

但如果再增加另一個第三方登入，則會破壞原本的結構。使用`OCP`的寫法會這樣。

``` java
interface ISocial {
    void login(Auth authData);
}

class Facebook implements ISocial {
    @Override
    public void login(Auth authData) {
        // 實作 facebook 登入
    }
}

class Google implements ISocial {
    @Override
    public void login(Auth authData) {
        // 實作 google 登入
    }
}

class OtherSocial implements ISocial {
    @Override
    public void login(Auth authData) {
        // 實作其他第三方登入
    }
}
```

接著重構`AuthController`

``` java
public class AuthController {
    public void login(ISocial social, Auth authData) {
        social.login(authData);
    }
}
```

如此一來，就算新增了其他第三方平台登入方式，也不會去改動到原本的程式碼