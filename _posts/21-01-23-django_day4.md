---
layout: post
title:  "Django 공부 4일차"
summary: Django에 대해 알아보자
author: GJ
date: '2021-01-25 16:58:23 +0530'
category: Web_Programming
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/network.jpg
---

 {% raw %} 

# Django

　

## 4일차

　

* 주석을 마저 달아보는 시간을 가져보자.
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

##### 모델📚

##### URL 🔧

　

#### Blog_App

　

##### 뼈대💀

* settings.py의 INSTALLED_APPS에 `'blog.apps.BlogConfig'`를 추가하자

　

##### 모델📚

　

**models.py**

```python
from django.db import models
from django.urls import reverse

# Create your models here.

class Post(models.Model):
    title = models.CharField('TITLE', max_length=50)
    slug = models.SlugField('SLUG', unique=True, allow_unicode=True, help_text='one word for title alias')
    descrption = models.CharField('DESCRIPTION', max_length=100, blank=True, help_text='simple description text')
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
        return reverse('blog:post_detail',args=(self.slug,))
    
    def get_previous_post(self):
        return self.get_previous_by_modify_date()

    def get_next_post(self):
        return self.get_next_by_modify_date()
```

　

**admin.py**

```python
from django.contrib import admin
from blog.models import Post
# Register your models here.

class PostAdmin(admin.ModelAdmin):
    list_display = ('title','modify_date')
    list_filter = ('modify_date','create_date')
    search_fields = ('title','content')
    prepopulated_fields = {'slug':('title',)}

admin.site.register(Post,PostAdmin)
```

* list_filter는 테이블의 column을 filtering 할 요소들을 넣을 수 있다.
* search_fields에 추가한 요소들을 기준으로 검색을 하게 된다.
* prepopulated_fields는 slug 필드를 title로 미리 채워주는 기능을 한다.

　

##### URL 🔧

**post_all.html**

```html
<h1>Blog List</h1>

{% for post in posts %}
    {{ post.get_absolute_url }}
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
            <a href="?page={{ page_obj.next_page_number }}">NextPahe</a> 
        {% endif %}
    </span>
</div>
```

* model에서 작성했던 get_absolute_url함수가 사용된다. 간단히 설명하자면 해당하는 detail_url을 return하는 함수이다.
* 버튼을 누르면 상세뷰로 넘어가게 된다.

　

**post_detail.html**

```html
<h2>{{ object.title }}</h2>

<p class="other_posts">
    {%if object.get_previous_by_modify_date %}
        <a href="{{ object.get_previous_post.get_absolute_url }}" title="View Previous Post"> &laquo;--{{ object.get_previous_post }}</a>
    {% endif %}

    {%if object.get_next_by_modify_date %}
        <a href="{{ object.get_next_post.get_absolute_url }}" title="View Next Post"> {{ object.get_Next_post }}--&raquo;</a>
    {% endif %}
</p>

<p class="date">{{ object.modify_date|date:"j F Y" }}</p>
<br/>

<div class="body">
    {{ object.content|linebreaks }}
</div>
```

* 마찬가지로 model에서 작성한 get_previous_by_modify_date와 get_next_by_modify_date함수를 사용한다.
* 이를 활용하여 이전, 이후 post와 링크를 연결한다.

##### 　

**post_archive.html**

```html
<h1>Post Archives until {% now -"N d, Y" %}</h1>
<ul>
    {% for date in date_list %}
        <li style="display:inline;">
            <a href="{% url 'blog:post_year_archive' date|date:'Y' %}">Year-{{ date|date:"Y" }}</a>
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

* {% now %}는 현재의 날짜와 시간을 출력한다. 이 때 'N d, Y'라는 것은

  January 25, 2021과 같은 형식으로 출력하라는 의미이다.

* 'blog:post_year_archive' date|date:'Y'는 blog의 post_year_archive라는 page에

  date|date:'Y'라는 인자를 넘기라는 의미이다.

* date|date:'Y'는 연도만 출력한다는 의미이다.

　

 {% endraw %} 

