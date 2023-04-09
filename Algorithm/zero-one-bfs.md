# 0-1 BFS
가중치가 0과 1인 그래프에서의 BFS(최단경로 알고리즘). 0과 1로만 구성된 가중치 그래프의 다익스트라 알고리즘을 최적화한 알고리즘이다.

# 과정
1. 덱의 front에서 노드를 꺼낸다.
2. 연결된 인접 노드를 살펴본다.
3. (현재 노드까지 cost + 인접 노드 가중치 < 지금까지 계산된 인접 노드까지 cost)를 만족하면 그 인접 노드까지 cost를 업데이트.
4. 3번에서 업데이트를 했을 경우, 현재 노드에서 해당 노드까지 가중치가 0이면 덱의 front에, 1이면 덱의 back에 삽입.
5. 덱이 빌 때까지 위 과정 반복.

이제 위 과정에 대한 설명을 시작하겠다. 일반적인 BFS는 가중치가 무조건 1이기 때문에 따로 신경써야 할 것은 없었다.

그러나 0과 1의 가중치만 존재한다면 단순 방문 횟수뿐만 아니라 가중치가 더 낮은 경우를 우선해야 한다.
즉, 가중치가 낮은(0) 정점으로의 이동을 높은 우선 순위로 두어야 하므로, 큐의 맨 앞에 두어야 한다.

이에 따라 BFS와 달리 덱을 사용해 시간 복잡도를 간단히 한다.

아래는 해당 알고리즘을 구현한 의사 코드. (python 기반)

```python
for all v in verticces:
        dist[v] = inf
    dist[start] <- 0
    deque d
    d.push_front(start)

    while d.empty() == false:
        vertex = get front element and pop as in BFS
        for all edges e of form(vertex, u):
            if travelling e relaxes distance to u:
                relax dist[u]
                if e.weight = 1:
                    d.push_back(u)
                else:
                    d.push_front(u)
```

# 시간 복잡도
탐색 시에 같은 정점을 또 방문하거나 같은 간선을 또 방문하는 일은 없다.

위 과정에서 간선을 전부 탐색한다 하면 O(E). 모든 정점들을 방문한다고 하면 O(V). 
따라서 시간 복잡도는 <span style="font-size:20px">**O(V + E)**</span>이다.

다익스트라 알고리즘의 시간 복잡도는 O(ElogE) 또는 O(ElogV)이기에 대게 더 빠르다.

# 참조
* [0-1 너비 우선 탐색](https://jiuge-meogari-ps.tistory.com/45)
* [0-1 BFS](https://chan9.tistory.com/120)
* [[algorithm] 0-1 BFS](https://velog.io/@nmrhtn7898/ps-0-1-BFS)
* [0-1 BFS(0-1 Breadth First Search)](https://nicotina04.tistory.com/168)
* [[그래프] 0-1 BFS 알고리즘](https://justicehui.github.io/medium-algorithm/2018/08/30/01BFS/)
- - -
#### 최초 생성: 2023-04-09