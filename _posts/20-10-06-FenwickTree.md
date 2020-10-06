---
layout: post
title:  "FenwickTree 대한 밍기적"
summary: 알고리즘 공부
author: GJ
date: '2020-10-06 16:10:23 +0530'
category: algorithm
comment: true
tags: [algorithm, FenwickTree, ming-gi-jeog]
thumbnail: /assets/img/posts/FenwickTree.jpg
---


## [Problem](https://www.acmicpc.net/problem/11659)

* 문제를 읽기 귀찮은 분들을 위해 간단하게 설명을 해보겠다.

    크기 N의 배열이 있다. M번의 query가 주어지고 각 쿼리에는 시작 index와 끝 index가 있다.
    쿼리에서 주어진 구간에서 주어진 배열의 부분합을 구하시오

* 가장 무식하고 단순하게 풀면 이렇다.

```cpp
#include <iostream>

using namespace std;

int N, M, from, to;
int arr[100001];
int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	cin >> N >> M;
	for (int i = 0; i < N; i++)
	{
		cin >> arr[i];
	}
	for (int i = 0; i < M; i++)
	{
		int sum = 0;
		cin >> from >> to;
		for (int j = from; j <= to; j++)
		{
			sum += arr[j - 1];
		}
		cout << sum << '\n';
	}

	return 0;
}
```

![TLE](https://drive.google.com/uc?export=view&id=1OVaZH7xUxh3YmubbE43FJfP4gPAO0cMF)

### 현실은 냉정한 법이었다. 그래서 준비했다.

# `FenwickTree`

    배열의 부분합을 빠르게 구할 수 있는 자료구조이다.


> 百聞不如一見(그냥 써보고 싶었다...)

### A라는 배열이 있다고 가정하자.

| A[1] | A[2] | A[3] | A[4] | A[5] | A[6] | A[7] | A[8] | A[9] | A[10] | A[11] | A[12] | A[13] | A[14] | A[15] | A[16] |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 2 | 16 | 1 | 3 | 8 | 4 | 12 | 7 | 5 | 13 | 6 | 9 | 11 | 10 | 14 | 15 |
| 2 |   | 1 |   | 8 |   | 12 |   | 5 |    | 6 |   | 11 |   | 14 |   |
| 18 |   |   |   | 12 |   |   |   | 18 |   |   |   | 21 |   |   |   |
| 22 |   |   |   |   |   |   |   | 33 |   |   |   |   |   |   |   |
| 53 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 136 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |

### 이를 FenwickTree에 다음과 같이 저장한다.

| F[1] | F[2] | F[3] | F[4] | F[5] | F[6] | F[7] | F[8] | F[9] | F[10] | F[11] | F[12] | F[13] | F[14] | F[15] | F[16] |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 |   | 3 |   | 5 |   | 7 |   | 9 |    | 11 |   | 13 |   | 15 |   |
| 2 |   |   |   | 6 |   |   |   | 10 |   |   |   | 14 |   |   |   |
| 4 |   |   |   |   |   |   |   | 12 |   |   |   |   |   |   |   |
| 8 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 16 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |

    왜 이렇게 고생해서 넣을 필요가 있는지 살펴보도록하자. 예를 들어, A라는 배열의 1~13까지의 부분합을 구하려한다.
    무식한 방법으로 구하면 13만큼의 시간 즉 구간의 길이만큼의 시간이 걸린다. 그런데 FenwickTree를 살펴보자.
    눈썰미가 좋은 사람은 눈치챘을지도 모른다. FenwickTree의 13번째 값과 12번째 값과 8번째 값을 합하게 되면
    1~13까지의 구간합을 마법처럼 구할 수 있다. 13 = 1101(2)
    F[8]은 1~8까지의 구간합을 F[12]는 9~12까지의 구간합을 F[13]은 13~13까지의 구간합을 갖고 있다.
    즉, 1~13의 구간합 Sum = F[1101(2)]+F[1100(2)]+F[1000(2)]

    이처럼 1~N까지의 구간합을 구하고 싶으면 N을 이진화 한 뒤 뒤에서부터 1을 없애주면서 FenwickTree의 값을 누적시키면 된다.
    이를 코드로 나타내면 다음과 같다.

```cpp
int sum(vector<int> &tree, int target)
{
    int ans = 0;
    while(target>0)
    {
        ans += tree[target];
        target -= (target & -target);       // 이 수식이 이해가 안 되면 2의 보수에 대해 알아보면 도움이 될 것이다.
    }                                       // x와 x의 2의 보수를 & 연산하게되면 x의 마지막 1만 남게 된다.
    return answer;
}
```

* 순서가 조금 거꾸로 된 느낌이지만 이제 FenwickTree를 만드는 함수를 작성해보자


```cpp
void update(vector<int> &tree, int target, int diff)
{
    while(target<tree.size())
    {
        tree[target] += diff;
        target += (target & -target);
    }
}
```

### FenwickTree는 Trie와는 반대로 익숙해지는건 금방이지만 이해는 쉽지가 않은 자료구조같다.
### 관련 문제로는 [Problem2](https://www.acmicpc.net/problem/2042)가 있다.

> [Problem solution](../../../../../solution/2020/10/06/baekjoon_11659)

> [Problem2 solution](../../../../../solution/2020/10/06/baekjoon_2042)
