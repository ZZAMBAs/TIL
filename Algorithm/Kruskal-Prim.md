# 크루스칼 알고리즘과 프림 알고리즘
MST를 생성하는 알고리즘.

- - -
##  크루스칼 알고리즘
간선을 기준으로 MST를 형성한다. 알고리즘 구현에서는 간선 연결 시, 사이클인지 항상 판단해야 된다.

###  과정
1. 노드들의 간선을 가중치를 기준으로 오름차순 정렬한다.
2. 가중치가 가장 작은 간선을 선택하고, 해당 간선에 연결된 두 정점이 서로 연결된 상태인지 확인한다.
3. &nbsp;
   1. 이미 연결된 상태이거나 사이클을 형성하면 해당 간선을 제외하고 1번으로 돌아간다.
   2. 연결된 상태가 아니고, 사이클을 형성하지 않는다면 둘을 연결하고 해당 간선을 제외한다. 
4. 모든 정점이 연결될 때까지 이 과정을 반복한다.

### 구현(C++)
Union-Find(Disjoint-Set)을 이용하여 구현한다. 프림 알고리즘과 달리 트리를 만들어가는 과정이 난잡하게 이루어지므로, 부모 판단이 중요하기 때문이다.

기본적으로 각 정점에 대한 Union-Find 연산이 있다고 가정한다.

```cpp
void kruskal(vector<pair<int, pair<int, int>>> E){ // { weight, { v1, v2 } }
    sort(E.begin(), E.end());
    for(pair<int, pair<int, int>> e : E){
        pair<int, int> vertex = e.second;
        int weight = e.first; // 필요 시 사용
        
        if( find(vertex.first) != find(vertex.second) )
            union(vertex.first, vertex.second);
    }
    return 0;
}
```

### 시간 복잡도
Union-Find의 시간 복잡도는 상수이므로, 간선을 정렬하는 시간 복잡도가 곧 크루스칼 알고리즘의 시간 복잡도가 된다.

가장 빠른 정렬 알고리즘의 시간 복잡도는 O(NlogN) 이므로, 크루스칼 알고리즘의 시간 복잡도는 <span style="font-size: 20px;">**O(ElogE)**</span> 이다.

- - -
## 프림 알고리즘
임의의 정점에서 시작하여 MST를 형성한다. 알고리즘 구현에서 사이클을 판단하지 않아도 된다.

### 과정
1. 임의의 정점을 트리 집합 T에 삽입한다. (이에 따라 T의 크기는 1이 된다.)
2. T에 속한 정점과 T에 속하지 않은 정점 사이 간선 중 가중치가 최소인 간선을 찾는다.
3. 찾는 간선을 연결하는 정점 중, T에 속하지 않은 정점을 T에 삽입한다. 
4. T에 모든 정점이 삽입될 때까지 2번부터 반복한다.

### 구현(C++)
다익스트라 알고리즘과 유사하게 구현. T에 속한 노드가 출발점인 간선들을 최소 힙에 넣으며 구현한다.

```cpp
#define PII pair<int,int>

vector<PII> ad[N];  // ad[i] : i 노드가 출발지인 간선들, { weight, destination }
priority_queue<PII, vector<PII>, greater<PII>> dist; // 최소 힙
bool selected[N]; // 이전에 선택된 노드인지 확인
    
int prim(int pn){ // pn: 노드 수
    int ret = 0;
    dist.push(PII(0,1));
    
    for(int i=1; i<=pn; i++){ // V
        int nowV = -1, min_dist = INT32_MAX;
        while(!dist.empty()){
            nowV = dist.top().second; // logV
            if(!selected[nowV]){
                min_dist = dist.top().first;
                break;
            }
            dist.pop();
        }
        if(min_dist == INT32_MAX) return INT32_MAX;
        ret += min_dist;
        selected[nowV] = true;
        for(auto edge: ad[node])
            dist.push(edge);
    
    }
    return ret;
}
```
위 코드는 예시일 뿐이며, 알고리즘을 제대로 이해했다면 다른 방식으로 코드를 작성해도 된다.

### 시간 복잡도
힙에 모든 정점(V)에 대해 최소 간선 값을 찾고(logV) + 해당 간선에 연결된 정점에 대하여, 다시 그 정점과 연결된 간선들(E)을 최소 힙에 넣어 업데이트(logV)하므로, O(VlogV + ElogV) -> <span style="font-size:20px;">**O(ElogV)**</span>다.

시간 복잡도는 우선순위 큐를 어떻게 구현함에 따라 달라질 수 있다.

## 크루스칼 VS 프림
시간 복잡도 차이에 따라 크루스칼 알고리즘은 노드 수가 많을 때, 프림 알고리즘은 간선 수가 많을 때 쓰는 것이 유용하다.
- - -
# 참조
* [Kruskal Algorithm and Prim Algorithm (크루스칼, 프림 알고리즘)](https://eehoeskrap.tistory.com/39)
* [크루스칼 알고리즘(Kruskal's Algorithm)](https://8iggy.tistory.com/160)
* [프림 알고리즘 ( Prim's algorithm )](https://www.weeklyps.com/entry/%ED%94%84%EB%A6%BC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Prims-algorithm)
- - -
#### 최초 생성: 2023-04-02
