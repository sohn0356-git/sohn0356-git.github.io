---
layout: post
title:  "Django 공부 1일차"
summary: Django에 대해 알아보자
author: GJ
date: '2021-01-21 18:08:23 +0530'
category: Web_Programming
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/network.jpg
---

# Django

　

## 1일차

　

* 설치에 이어 바로 Django를 학습해보겠다.
* 교재는 [파이썬 웹 프로그래밍: Django(장고)로 배우는 쉽고 빠른 웹 개발](https://www.hanbit.co.kr/store/books/look.php?p_code=B5790464800)를 참고해서 진행하겠다.

　

### 프로젝트 뼈대 만들기💀

　

#### 프로젝트 생성

* `django-admin startproject mysite`

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_1%EC%9D%BC%EC%B0%A8_01.JPG?raw=true">

　

* 명령어 하나로 무언가 많이 생긴 것을 볼 수 있다.

* 이번에는 애플리케이션을 생성해보자. 

  * `cd mysite`
  * `python manage.py startapp polls`


　

* 이번에도 자동으로 무언가 많이 생겼다. 이처럼 필요한 파일들은 장고가 알아서 생성해준다.

  개발자들은 그 내용을 채워넣기만 하면 된다. 👌

* 다음으로 settings.py를 손 봐야 한다.

　

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

* 여기서 'polls.apps.PollsConfig'를 추가해주자!

　

```python
# Database
# https://docs.djangoproject.com/en/3.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

* 장고는 sqlite3를 데이터베이스 엔진으로 사용한다. 혹시 MySQL이나 다른 데이터베이스로 변경하소 싶다면 이 부분을 고쳐주자!

　

```python
TIME_ZONE = 'UTC'
```

* 마지막으로 시간을 한국 시간으로 변경해보자!
  * TIME_ZONE = 'Asia/Seoul'

　

#### 기본 테이블 생성

* migrate

  * 데이터베이스에 변경사항이 있을 경우 이를 반영해주는 명령이다.

  * 앞으로 우리는 테이블을 생성하거나 지우면서 Model을 변경할 것이다.

    그럴 때마다 우리는 이 명령어를 사용하게 될 것이므로 잘 기억해두자!

  * `python manage.py migrate`

　

#### 작업 확인

* 아직 명령어를 몇 개 입력해보지 않았지만 지금까지 만들어진 것을 확인해보자.
* `python manage.py runserver 0.0.0.0:8000`
* 이제 웹 브라우저를 키고 [127.0.0.1:8000](127.0.0.1:8000)에 들어가보자.

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_1%EC%9D%BC%EC%B0%A8_02.JPG?raw=true">

　

### Model 코딩

---

**models.py**

```python
from django.db import models

# Create your models here.

class Question(models.Model):
    """
    Question 테이블
    """
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date pulished')

    def __str__(self):
        return self.question_text

class Choice(models.Model):
    """
    Choice 테이블
    """
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

　

지금 우리는 Question과 Choice라는 2개의 테이블을 생성한 것이다.

**Question**

| 테이블 컬럼명 | 컬럼 타입    | 장고의 클래스 변수 | 장고의 필드 클래스                     |
| ------------- | ------------ | ------------------ | -------------------------------------- |
| id            | integer      | (id)               | (PK는 장고에서 자동생성)               |
| question_text | varchar(200) | question_text      | models.CharField(max_length=200)       |
| pub_date      | datetime     | pub_date           | models.DateTimeField('date published') |

　

**Choice**

| 테이블 컬럼명 | 컬럼 타입    | 장고의 클래스 변수 | 장고의 필드 클래스               |
| ------------- | ------------ | ------------------ | -------------------------------- |
| id            | integer      | (id)               | (PK는 장고에서 자동생성)         |
| choice_text   | varchar(200) | choice_text        | models.CharField(max_length=200) |
| votes         | integer      | votes              | models.IntegerField(default=0)   |
| question_id   | integer      | question           | models.ForeginKey(Question)      |

---

**admin.py**

```python
from django.contrib import admin
from polls.models import Question, Choice
# Register your models here.

admin.site.register(Question)
admin.site.register(Choice)
```

---

　

* 이제 방금 만들었던 두 테이블을 Admin 사이트에서 볼 수 있게 되었다.

  여기서 2가지 작업이 필요하다.

* `python manage.py makemigrations`

  * polls/migrations 디렉토리 하위에 마이그레이션 파일들이 생성됨

* `python manage.py migrate`

  * 생성된 마이그레이션 파일들을 데이터베이스에 테이블로 옮겨줌

　

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_1%EC%9D%BC%EC%B0%A8_03.JPG?raw=true">

* 위와 같이 `python manage.py sqlmigrate polls 0001`로 SQL 문장을 확인 할 수 있다.

　

* Admin 사이트에서 현재까지의 작업을 보기 원한다면 먼저 Admin 아이디가 필요하다.
* `python manage.py createsuperuser`
* 이제 다시 `python manage.py runserver 0.0.0.0:8000`으로 현재 상황을 점검해보자.

　

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_1%EC%9D%BC%EC%B0%A8_04.JPG?raw=true">

* Choices와 Questions가 잘 들어가 있는 것을 확인할 수 있다.