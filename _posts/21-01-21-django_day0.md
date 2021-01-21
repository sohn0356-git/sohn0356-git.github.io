---
layout: post
title:  "Django 공부 0일차"
summary: Django에 대해 알아보자
author: GJ
date: '2021-01-20 16:08:23 +0530'
category: Web_Programming
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/newwork.jpg
---

# Django

　

## 0일차

　

* 오늘부터 Django를 학습해보겠다.
* 교재는 [파이썬 웹 프로그래밍: Django(장고)로 배우는 쉽고 빠른 웹 개발](https://www.hanbit.co.kr/store/books/look.php?p_code=B5790464800)를 참고해서 진행하겠다.
* 학습을 위해 먼저 가상환경을 구축해보자!
  * [Python](https://www.python.org/) 설치하기
  * [VS code](https://code.visualstudio.com/) 설치하기

　

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_0%EC%9D%BC%EC%B0%A8_01.JPG?raw=true">

* 설치가 끝났으면 가상환경을 구축해야한다.

　

### 가상환경 구축

    1. `ctrl` + `shift` + `` `을 눌러서 새로운 터미널을 켜자
    2. `python -m venv myvenv`를 입력하여 myvenv라는 가상환경을 구축하자
    3. `cd ./myvenv/Scripts`를 입력하여 디렉토리를 이동한 뒤
    4. `activate`를 입력하여 가상환경을 시작하자


　

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_0%EC%9D%BC%EC%B0%A8_02.JPG?raw=true">

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_0%EC%9D%BC%EC%B0%A8_03.JPG?raw=true">

* 잘 되었으면 가상환경에 들어온 표시로 그림처럼 `(myvenv)`라는 글자가 앞에 추가된다.
* 가상환경을 종료하고 싶으면 **같은 디렉토리에서** deactivate를 입력하면 된다.

　

### Django 설치

* `python -m pip install --upgrade pip`
* `pip install django`
* `pip list`

　

<img src="https://github.com/sohn0356-git/sohn0356-git.github.io/blob/master/_posts/md-images/django_0%EC%9D%BC%EC%B0%A8_04.JPG?raw=true">

　

* 위와 같이 잘 설치가 되었으면 이제 프로젝트를 시작할 수 있다.