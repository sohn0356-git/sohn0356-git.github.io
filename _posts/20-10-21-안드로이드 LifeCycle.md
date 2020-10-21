---
layout: post
title:  "안드로이드 LifeCycle에 대한 밍기적"
summary: 안드로이드 면접 대비 공부
author: GJ
date: '2020-10-21 15:12:23 +0530'
category: android
comment: true
tags: [ming-gi-jeog]
thumbnail: /assets/img/posts/android.png
---

#  　

네이버, 카카오 면접을 대비해 android 공부를 하며 작성하게 되었다.

Life Cycle에 관련된 내용으로 Activity가 실행될 때 어떤 메소드들이 호출되는지 함께 알아보자

먼저 전체 Life Cycle이다.

#  　

![Life Cycle](https://drive.google.com/uc?export=view&id=1Iudggcp-N_TBxGUEeI0F-ncMaNVkuf1U)


#  　

우리가 앱을 실행시키고 화면에 보이기까지 무려 3가지 메소드가 호출된다.

onCreate -> onStart -> onResume

onCreate

- 화면이 메모리에 만들어지고 여러 초기화 과정을 진행하게 된다.

onStart

- 화면 기능을 실행할 준비를 한다.

onResume

- activity가 화면 맨 앞에서(foreground) 동작 중인 상태로 사용자와 상호작용을 한다.

#  　

이후 Home 버튼이나 Activity가 종료될 때 3가지 메소드가 호출된다.

onPause -> onStop -> onDestroy

onPause

- 사용자가 activity를 벗어나게 되면 제일 먼저 호출되는 메소드이다.

부분적으로 activity가 가려진 상태로 메모리가 부족하면 강제 종료될 수도 있다.

onStop

- 이번에는 activity가 완전히 가려진 상태이다. 메모리가 부족하면 우선적으로 강제종료된다.

onDestroy

- Activity가 소멸되기 전에 호출된다

onStop 상태에서 다시 activity로 돌아갈 경우 onRestart라는 메소드를 호출하게 된다.

#  　

이렇게 간략하게 7가지 메소드를 살펴보았다. 그러나 살펴보는 것만으로는 머리에 남지 않는다.

그래서 준비했다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    Log.i("MainActivity","This is onCreate Method");
}
```

이런 식으로 7가지 메소드에 대해서 전부 Log를 남긴다.

참고로 Log에는 5가지 종류가 있다.(Log.e(), Log.w(), Log.i(), Log.d(), Log.v())

`error`, `warning`, `info`, `debug`, `verbose`

앱을 실행시키면 다음과 같이 나온다.

    2020-10-21 15:35:15.479 17645-17645/com.example.lifecycle I/MainActivity: This is onCreate Method
    2020-10-21 15:35:15.481 17645-17645/com.example.lifecycle I/MainActivity: This is onStart Method
    2020-10-21 15:35:15.483 17645-17645/com.example.lifecycle I/MainActivity: This is onResume Method

이제 종료시켜보자.

    2020-10-21 15:35:55.659 17645-17645/com.example.lifecycle I/MainActivity: This is onPause Method
    2020-10-21 15:35:56.231 17645-17645/com.example.lifecycle I/MainActivity: This is onStop Method
    2020-10-21 15:35:56.231 17645-17645/com.example.lifecycle I/MainActivity: This is onDestroy Method

우리가 배운 그대로이다. 그렇다면 Home버튼을 누르면 어떻게 될까?

    2020-10-21 15:36:34.845 17899-17899/com.example.lifecycle I/MainActivity: This is onPause Method
    2020-10-21 15:36:34.860 17899-17899/com.example.lifecycle I/MainActivity: This is onStop Method

다시 앱을 실행시켜보자.

    2020-10-21 15:37:16.660 17899-17899/com.example.lifecycle I/MainActivity: This is onRestart Method
    2020-10-21 15:37:16.661 17899-17899/com.example.lifecycle I/MainActivity: This is onStart Method
    2020-10-21 15:37:16.663 17899-17899/com.example.lifecycle I/MainActivity: This is onResume Method

#  　

대충 어떤 느낌인지 감이 오는가? 그러나 여기서 만족하면 안 된다.

우리가 생각나는 것은 다 해봐야한다. 만약 화면을 회전시키면 어떻게 될까? 

한 번 예상해보고 답을 보자! 놀랍게도 다음과 같다.

    2020-10-21 15:39:28.862 17899-17899/com.example.lifecycle I/MainActivity: This is onPause Method
    2020-10-21 15:39:28.874 17899-17899/com.example.lifecycle I/MainActivity: This is onStop Method
    2020-10-21 15:39:28.874 17899-17899/com.example.lifecycle I/MainActivity: This is onDestroy Method
    2020-10-21 15:39:28.957 17899-17899/com.example.lifecycle I/MainActivity: This is onCreate Method
    2020-10-21 15:39:28.961 17899-17899/com.example.lifecycle I/MainActivity: This is onStart Method
    2020-10-21 15:39:28.965 17899-17899/com.example.lifecycle I/MainActivity: This is onResume Method

#  　

간단히 설명하자면 가로 화면에서 세로 화면으로 바뀌면 기존의 Activity를 다 지우고 새로 만들게 된다.

위의 3개의 메소드는 가로 화면을 지우기 위해 밑에 3개의 메소드는 세로 화면을 새롭게 만들기 위해 호출되었다.

그러면 여기서 문제! 다음과 같이 +버튼을 누르면 숫자가 올라가는 앱을 개발했다고 치자.

![Life Cycle](https://drive.google.com/uc?export=view&id=1P53iLeB2D-QT51Ir1401nFanJSnIyK0i)

#  　

![Life Cycle](https://drive.google.com/uc?export=view&id=1rm6rfFG_ZZ6Wsj62XZf1ca6_U5jQa5eu)

#  　

숫자를 열심히 올리고 화면을 회전시키면 어떻게 될까? 결과는 다음과 같다.

#  　

![Life Cycle](https://drive.google.com/uc?export=view&id=1mjEyolQME4PfhvWeec3TB9DhZsoe3XYY)

#  　

화면을 돌리니 기존의 data가 다 날아가서 너무 슬프다...ㅠㅠ

이럴 때를 대비해서 똑똑한 선배들이 만들어둔 메소드들이 있다.

- protected void onSaveInstanceState(@NonNull Bundle outState)

- protected void onRestoreInstanceState(@NonNull Bundle savedInstanceState)

onPause 메소드가 호출된 다음 onSaveInstanceState를 호출하여 data를 저장한다. 저장하는 방식은

Intent와 비슷하다. 그 후 onStart 메소드가 호출된 뒤 onRestoreInstanceState가 호출되면서 기존의 데이터를 유지할 수 있게 된다.

전체 코드를 보면서 한 번씩 따라해보면 면접 때까지는 기억에 남을 것이다.


```java
package com.example.lifecycle;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    Button bt_next;
    Button bt_plus;
    TextView tv_cnt;
    int cnt;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        bt_next = (Button)findViewById(R.id.bt_next);
        bt_plus = (Button)findViewById(R.id.bt_plus);
        tv_cnt = (TextView)findViewById(R.id.tv_cnt);
        bt_next.setOnClickListener(onClickListener);
        bt_plus.setOnClickListener(onClickListener);

        Log.i("MainActivity","This is onCreate Method");
    }


    View.OnClickListener onClickListener = new View.OnClickListener(){
        @Override
        public void onClick(View v) {
            switch (v.getId()){
                case R.id.bt_next:
                    Intent intent = new Intent(MainActivity.this, MainActivity2.class);
                    startActivity(intent);
                    break;
                case R.id.bt_plus:
                    cnt = Integer.parseInt(tv_cnt.getText().toString())+1;
                    tv_cnt.setText(String.valueOf(cnt));
                    break;
            }

        }
    };

    @Override
    protected void onStart() {
        super.onStart();
        Log.i("MainActivity","This is onStart Method");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        Log.i("MainActivity","This is onRestart Method");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i("MainActivity","This is onStop Method");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.i("MainActivity","This is onDestroy Method");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.i("MainActivity","This is onPause Method");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i("MainActivity","This is onResume Method");
    }

    @Override
    protected void onSaveInstanceState(@NonNull Bundle outState) {
        super.onSaveInstanceState(outState);
        Log.i("MainActivity","This is onSaveInstanceState Method");
        outState.putInt("count",cnt);
    }

    @Override
    protected void onRestoreInstanceState(@NonNull Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
        Log.i("MainActivity","This is onRestoreInstanceState Method");
        cnt = savedInstanceState.getInt("count");
        tv_cnt.setText(String.valueOf(cnt));
    }
}
```