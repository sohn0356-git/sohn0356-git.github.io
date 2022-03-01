---
layout: post
title:  "22-03-01 개발일기"
summary: react-native 개발일기
author: GJ
date: '2022-03-01 11:55:23 +0530'
category: Development
comment: true
tags: [TIL]
thumbnail: /assets/img/posts/network.jpg
---

# ScrollView

```js
<ScrollView
    onScroll={({nativeEvent})=>onchange(nativeEvent)}
    showsHorizontalScrollIndicator={false}
    pagingEnabled
    horizontal
    scrollEventThrottle={0}
    style={styles.wrap}
>
```

- onScroll : Scroll Event 발생 시 호출
- pagingEnabled : page 단위로 스크롤 됨
- scrollEventThrottle : 스크롤의 민감도(default=0), 낮을수록 민감도는 낮아지고 속도는 올라감