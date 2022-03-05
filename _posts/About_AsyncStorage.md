---
layout: post
title:  "About_ScrollView"
summary: About_ScrollView
author: GJ
date: '2022-03-03 21:55:23 +0530'
category: Development
comment: true
tags: [TIL]
thumbnail: /assets/img/posts/network.jpg
---

# ScrollView

```js
import AsyncStorage from '@react-native-community/async-storage';

saveStorage = () => {
    AsyncStorage.setItem("LOGIN", JSON.stringify({id:"id01", pw:"pw01"}), () => {
        console.log("save data")
    });
}
readStorage = () => {
    AsyncStorage.getItem("LOGIN", (err,result) => {
        console.log(result);
    });
}
removeStorage = () => {
    AsyncStorage.removeItem("LOGIN", ()=>{
        console.log("remove data")
    });
}
```

- Key와 Value를 사용하여 데이터를 저장
- App을 껏다가 켜도 데이터가 남아있음
- Value에는 문자열 혹은 JSON.stringify 함수를 사용하여 객체나 배열을 저장할 수 있다.