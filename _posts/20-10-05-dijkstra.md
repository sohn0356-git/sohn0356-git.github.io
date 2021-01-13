---
layout: post
title:  "DIJKSTRA에 대한 밍기적"
summary: Graph 알고리즘 공부
author: GJ
date: '2020-10-05 15:04:23 +0530'
category: Algorithm
comment: true
tags: [algorithm, dijkstra, ming-gi-jeog]
thumbnail: /assets/img/posts/dijkstra.jpg
---


# `DIJKSTRA란?`

#  　

다익스트라 알고리즘은 그래프의 한 정점에서 나머지 정점까지의 최소비용을 빠르게 계산하는 알고리즘이다.

이름과 다르게 방법은 무척 간단하다.

1. 방문하지 않은 정점 중에서 cost가 가장 작은 정점 v를 선택한다.
2. v와 연결된 정점들을 돌면서 cost의 최솟값을 갱신한다.

#  　


* 먼저 간선의 정보를 저장할 `struct`를 만들어보자


```cpp
struct Edge{
    int to;
    int cost;
    Edge(int _to, int _cost): to(_to), cost(_cost){}
};
```

#  　

* 다음으로 두 가지 정보를 저장할 배열이 필요하다.
    1. 방문한 정점인지 아닌지 확인하는 배열
    2. 최소 비용을 저장하는 배열

#  　


```cpp
bool visited[N_vertex];
int min_cost[N_vertex];         // 이 때 min_cost는 INF로 초기화를 해주어야한다.
```

#  　

* 준비물을 다 챙겼으니 이제 코드를 작성해보자

#  　

```cpp
min_cost[start] = 0;
priority_queue<pair<int,int>>pq;
pq.push({-min_cost[start],start});
while(!pq.empty())
{
    pair<int,int> cur = pq.top();
    pq.pop();
    int cur_vertex = cur.second;
    if(visited[cur_vertex])
    {
        continue;
    }
    visited[cur_vertex] = true;
    for(int i=0;i<graph[cur_vertex].size();i++)
    {
        Edge next = graph[cur_vertex][i];
        if(min_cost[next.to]>min_cost[cur_vertex]+next.cost)
        {
            min_cost[next.to] = min_cost[cur_vertex] + next.cost;
            pq.push({-min_cost[next.to],next.to});
        }
    }
}
```

* `DIJKSTRA` 알고리즘을 정복했다. 이를 활용해서 문제를 풀고 싶다면 [Problem](https://www.acmicpc.net/problem/1753)를 들어가 보기를 바란다.

#  　

#### 그런데 이렇게 끝내기에는 뭔가 아쉽다는 생각이 든다.

### 그래서 [Problem2](https://www.acmicpc.net/problem/11779)를 준비했다.

#  　



똑똑한 여러분이라면 이제 `DIJKSTRA`정도는 쉽게 풀 것이다. 그러나 DP 문제에서도 그렇지만

최소가 되는 경로를 출력하라고 하면 익숙하지 않은 사람도 분명 있을 것이라 생각한다.

Stack을 활용하면 이를 멋드러지게 해결할 수 있어서 이 방법을 소개하려고 한다.

#  　

1.stack과 배열을 준비한다.


```cpp
#include <stack>
int starting_point[N_vertex];   //최단 경로를 추적하기 위한 녀석으로 -1로 전부 초기화 해주자!
```

#  　


2.우리는 정점을 한 개씩 방문하면서 cost를 갱신해주는 작업을 해주었다. 이 때 Code 한 줄을 끼어넣어보자

#  　


```cpp
if(min_cost[next.to]>min_cost[cur_vertex]+next.cost)
{
    min_cost[next.to] = min_cost[cur_vertex] + next.cost;
    pq.push({-min_cost[next.to],next.to});
    starting_point[next.to] = cur_vertex;
}
```

#  　


3.마지막으로 이를 출력해보자

#  　


```cpp
stack<int>s;
int prev = destination; //최종 목적지에서 start까지 역추적을 할 것이다.
while(prev!=-1)
{
    s.push(prev);
    prev = starting_point[prev];
}
```

> [Problem solution](../../../../../solution/2020/10/05/백준-1753)

> [Problem2 solution](../../../../../solution/2020/10/05/백준-11779)

---
## The end
