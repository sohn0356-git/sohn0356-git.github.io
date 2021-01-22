---
layout: post
title:  "Django 공부 2일차"
summary: Django에 대해 알아보자
author: GJ
date: '2021-01-22 18:08:23 +0530'
category: Web_Programming
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/network.jpg
---
 {% raw %} 
# Django

　

## 2일차

　

* 어제에 이어 바로 Django를 학습해보겠다.
* 교재는 [파이썬 웹 프로그래밍: Django(장고)로 배우는 쉽고 빠른 웹 개발](https://www.hanbit.co.kr/store/books/look.php?p_code=B7703021280)를 참고해서 진행하겠다.
* 오늘은 다양한 예제를 통해 app 만드는 작업에 익숙해져보자!

　

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

* `cd mysite2`
* `python manage.py migrate`
* `python manage.py startapp bookmark`

　

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

　

#### Blog_App

　

##### 뼈대💀

* `python manage.py startapp blog`

　

##### 모델📚

**models.py**

```python
from django.db import models
from django.urls import reverse

# Create your models here.
class Post(models.Model):
    title = models.CharField('TITLE', max_length=50)
    slug = models.SlugField('SLUG', unique=True, allow_unicode=True, help_text='one word for title alias.')
    description = models.CharField('DESCRIPTION', max_length=100, blank=True,help_text='simple description text')
    content = models.TextField('CONTENT')
    create_date = models.DateTimeField('Create Date', auto_now_add=True)
    modify_date = models.DateTimeField('Modify Date', auto_now=True)

    class Meta:
        verbose_name = 'post'
        verbose_name_plural = 'posts'
        db_table = 'my_post'
        ordering = ('-modify_date',)

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('blog:post_detail', args=(self.slug,))

    def get_previous_post(self):
        return self.get_previous_by_modify_date()

    def get_next_post(self):
        return self.get_next_by_modify_date()
```

　

---

　

**admin.py**

```python
from django.contrib import admin
from blog.models import Post

# Register your models here.

class PostAdmin(admin.ModelAdmin):
    list_display = ('title', 'modify_date')
    list_filter = ('modify_date',)
    search_fields = ('title','content')
    prepopulated_fields = {'slug': ('title',)}

admin.site.register(Post, PostAdmin)
```

　

##### URL 🔧

**urls.py**

```python
from django.conf.urls import url
from blog.views import *

urlpatterns = [
    url(r'^$', PostLV.as_view(), name='index'),
    url(r'^post/$',PostLV.as_view(),name='post_lsit'),
    url(r'^post/(?P<slug>[-\w]+)/$', PostDV.as_view(), name='post_detail'),
    url(r'^archieve/$', PostAV.as_view(), name='post_archieve'),
    url(r'^(?P<year>\d{4})/$', PostYAV.as_view(), name='post_year_archieve'),
    url(r'^(?P<year>\d{4}/(?P<month>[a-z]{3})/$',PostMAV.as_view(), name='post_month_archieve'),
    url(r'^(?P<year>\d{4}/(?P<month>[a-z]{3})/(?P<day>{1,2})/$',PostDAV.as_view(), name='post_day_archieve'),
    url(r'^today/$', PostTAV.as_view(), name='post_today_archieve'),
]
```

　

**views.py**

```python
from django.conf.urls import url
from blog.views import *

app_name = 'blog'

urlpatterns = [
    url(r'^$', PostLV.as_view(), name='index'),
    url(r'^post/$',PostLV.as_view(),name='post_lsit'),
    url(r'^post/(?P<slug>[-\w]+)/$', PostDV.as_view(), name='post_detail'),
    url(r'^archive/$', PostAV.as_view(), name='post_archive'),
    url(r'^(?P<year>\d{4})/$', PostYAV.as_view(), name='post_year_archive'),
    url(r'^(?P<year>\d{4})/(?P<month>[a-z]{3})/$',PostMAV.as_view(), name='post_month_archive'),
    url(r'^(?P<year>\d{4})/(?P<month>[a-z]{3})/(?P<day>\d{1,2})/$',PostDAV.as_view(), name='post_day_archive'),
    url(r'^today/$', PostTAV.as_view(), name='post_today_archive'),
]
```

　

**post_all.html**

```html
<h1>Blog List</h1>
{% for post in posts %}
    <h2><a href='{{ post.get_absolute_url }}'>{{ post.title }}</a></h2>
    {{ post.modify_date|date:"N d, Y" }}
    <p>{{ post.description }}</p>
{% endfor %}

<br/>

<div>
    <span>
        {% if page_obj.has_previous %}
            <a href="?page={{ page_obj.previous_page_number }}">PreviousPage</a>
        {% endif %}

        Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

        {% if page_obj.has_next %}
            <a href="?page={{ page_obj.next_page_number }}">NextPage</a>
        {% endif %}
    </span>
</div>
```

　

**post_detail.html**

```html
<h2>{{ object.title }}</h2>
<p class="other_posts">
    {% if object.get_previous_by_modify_date %}
    <a href="{{ object.get_previous_post.get_absolute_url }}" title="View previous post"> &laquo;--{{ object.get_previous_post }}</a>
    {% endif %}

    {% if object.get_next_by_modify_date %}
    <a href="{{ object.get_next_post.get_absolute_url }}" title="View next post"> {{ object.get_next_post }}--&raquo;</a>
    {% endif %}
</p>

<p class="date">{{ object.modify_date|date:"j F Y" }}</p>
<br/>

<div class="body">
    {{ object.content|linebreaks }}
</div>
```

　

**post_archive.html**

```html
<h1>Post Archive until {% now "N d, Y" %}</h1>
<ul>
    {% for date in date_list %}
    <li style="display: inline;">
        <a href="{% url 'blog:post_year_archive' date|date:'Y' %">Year-{{ date|date:"Y" }}</a>
    </li>
    {% endfor %}
</ul>
<br/>

<div>
    <ul>
        {% for post in object_list %}
        <li>{{ post.modify_date|date:"Y-m-d" }}&nbsp;&nbsp;
            <a href="{{ post.get_absolute_url }}"><strong>{{ post.title }}</strong></a>
        </li>
        {% endfor %}
    </ul>
</div>
```

　

**post_archive_year.html**

```html
<h1>Post Archive for {{ year|date:"Y" }}</h1>
<ul>
    {% for date in date_list %}
    <li style="display: inline;">
        <a href="{% url 'blog:post_year_archive' date|date:'Y' date|date:'b' %">{{ date|date:"F" }}</a>
    </li>
    {% endfor %}
</ul>
<br/>

<div>
    <ul>
        {% for post in object_list %}
        <li>{{ post.modify_date|date:"Y-m-d" }}&nbsp;&nbsp;&nbsp;
            <a href="{{ post.get_absolute_url }}"><strong>{{ post.title }}</strong></a>
        </li>
        {% endfor %}
    </ul>
</div>
```

　

**post_archive_month.html**

```html
<h1>Post Archive for {{ month|date:"N, Y" }}</h1>

<div>
    <ul>
        {% for post in object_list %}
        <li>{{ post.modify_date|date:"Y-m-d" }}&nbsp;&nbsp;&nbsp;
            <a href="{{ post.get_absolute_url }}"><strong>{{ post.title }}</strong></a>
        </li>
        {% endfor %}
    </ul>
</div>
```

　

**post_archive_day.html**

```html
<h1>Post Archive for {{ month|date:"N d, Y" }}</h1>

<div>
    <ul>
        {% for post in object_list %}
        <li>{{ post.modify_date|date:"Y-m-d" }}&nbsp;&nbsp;&nbsp;
            <a href="{{ post.get_absolute_url }}"><strong>{{ post.title }}</strong></a>
        </li>
        {% endfor %}
    </ul>
</div>
```



* 다음엔 위 코드를 분석해보는 시간을 가져보자.
 {% endraw %} 
