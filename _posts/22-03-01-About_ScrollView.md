---
layout: post
title:  "About_ScrollView"
summary: About_ScrollView
author: GJ
date: '2022-03-01 14:55:23 +0530'
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

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/assets/img/posts/220301_diary_01.png?raw=true">
