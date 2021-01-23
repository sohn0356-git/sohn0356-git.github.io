---
layout: post
title:  "Django 공부 3일차"
summary: Django에 대해 알아보자
author: GJ
date: '2021-01-23 16:58:23 +0530'
category: Web_Programming
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/network.jpg
---

 {% raw %} 

# Django

　

## 3일차

　

* 어제 배운 내용에 주석을 달아보는 시간을 가지려 한다.
* 교재는 [파이썬 웹 프로그래밍: Django(장고)로 배우는 쉽고 빠른 웹 개발](https://www.hanbit.co.kr/store/books/look.php?p_code=B7703021280)를 참고해서 진행하겠다.

　

### 실습

> 목차
>
> * [Bookmark_App](#Bookmark_App)
> * [Blog_App](#Blog_App)

　

#### Template_App

　

##### 뼈대💀

##### 모델📚

##### URL 🔧

　

#### Bookmark_App

　

##### 뼈대💀

* `django-admin startproject mysite2`　
* mysite2라는 프로젝트를 생성한다.

* `cd mysite2`

* `python manage.py migrate`

* `python manage.py startapp bookmark`

  * bookmark라는 app을 생성한다.

* 이 후 우리는 settings에 들어가서  INSTALLED_APPS라는 리스트에 'bookmark.apps.BookmarkConfig'를 꼭 추가해주어야 한다.

  templates경로 추가와 static경로 추가도 잊지 말자!

　

##### 모델📚

**models.py**

```python
from django.db import models

# Create your models here.
class Bookmark(models.Model):
    title = models.CharField(max_length=100,blank=True,null=True)
    url = models.URLField('url',unique=True)

    def __str__(self):
        return self.title
```

* Bookmark라는 테이블을 만들었다. 구성요소로는 title과 url이 있다. 여기서 title의 blank=True는 빈 채로 저장되는 것을 허용하는 것이고

  null=True는 NULL로 저장되는 것을 허용하는 것이다.

* unique=True는 동일한 정보를 넣는 것을 금지하는 것이다. 실제로 admin 페이지에서 동일한 url을 넣으려 했으나 거절당했다😥

* def _ _ str _ _(self): 는 매우 중요한 역할을 한다. 이는 URL쪽에서 다루도록 하겠다.

　

---

　

**admin.py**

```python
from django.contrib import admin
from bookmark.models import Bookmark
# Register your models here.

class BookmarkAdmin(admin.ModelAdmin):
    list_display = ('title','url')

admin.site.register(Bookmark, BookmarkAdmin)
```

* admin.site.register를 하면 admin페이지에서 Bookmark를 볼 수 있게 된다.
* list_display는 튜플이 들어가며 여기에 열거한 데이터들이 admin페이지에서 출력된다.

　

##### URL 🔧

**urls.py**

```python
from django.conf.urls import url
from bookmark.views import BookmarkLV, BookmarkDV

app_name = 'bookmark'

urlpatterns = [
    url(r'^$',BookmarkLV.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', BookmarkDV.as_view(), name='detail'),
]
```

* 정규표현식( ^:시작 $:끝 )

  * r'[0-9]' => 0~9의 숫자 한글자	r'[a-z]' => 소문자 1글자	r'[a-zA-Z] 대소문자 1글자

  * r'\d' => 숫자 한글자	r'\d+' => 숫자 1글자 이상	r'\d{m} => 숫자 m글자	r'\d{m,n}' => 숫자 m글자 이상 n글자 이하

  * r'\w' => word를 표현하며 알파벳 | 숫자 | _ 중의 한글자

  * r'(?P) => 다음 정규표현식을 적용해보자. 조건(이 경우 숫자 1글자 이상)을 만족한다면	<pk> => pk라는 변수명으로 인자를

    BookmarkDV.as_view()에 넘기겠다.

　

**views.py**

```python
from django.shortcuts import render
from django.views.generic import ListView, DetailView
from bookmark.models import Bookmark

# Create your views here.

class BookmarkLV(ListView):
    template_name = 'bookmark_list.html'
    model = Bookmark

class BookmarkDV(DetailView):
    template_name = 'bookmark_detail.html'
    model = Bookmark
```

* template_name의 경우 listview는 default가 () _list.html detailview는 () _detail.html이다.

　

**bookmark_list.html**

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Django Bookmark List</title>
    </head>
    <body>
        <h1>Bookmark</h1>
        <ul>
            {% for bookmark in object_list %}
                <li><a href="{% url 'detail' bookmark.id %}">{{ bookmark }}</a></li>
            {% endfor %}
        </ul>
    </body>
</html>
```

* 아까 _ _ str _ _ () 함수를 작성한 이유가 여기서 나온다. 우리는 화면에 naver라는 title이 출력되길 원하면서 {{ bookmark }}를 작성했지만

  _ _ str _ _ ()함수가 없다면 naver대신 Bookmark object(1)이라는 끔찍한 출력을 보게 될 것이다.

　

**bookmark_detail.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Django Bookmark Detail</title>
</head>
<body>
    <h1>{{ object.title }}</h1>
    <ul>
        <li>URL: <a href="{{ object.url }}">{{ object.url }}</a></li>
    </ul>
</body>
</html>
```

　

 {% endraw %} 