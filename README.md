# codingtest_programmers_the-farthest-node
BFS로 풀이하고 두가지 방법 이용

문제 : https://programmers.co.kr/learn/courses/30/lessons/49189

### 풀이 법

1. 자료구조는 인접 행렬을 표시하는 딕셔너리와, 노드1과 다른 노드 사이의 거리를 표시하는 딕셔너리로 구성
2. 간선의 연결 정보를 인접행렬에 담아줌
3. 현재 노드에 연결되어 있는 모든 노드에 대해 bfs 수행
4. 가장 거리가 먼 노드의 갯수를 카운트

### KEY POINT

문제를 접근한 방법은 크게 두 가지이다. 첫번째는 인접 행렬을 만들 때 이차원 리스트로 만들고 연결이 된 경우에 1, 안된 경우 0으로 나타내었다. 

* 틀린 코드 - 시간 초과

  ``` python
  from collections import deque
  
  def solution(n, edge):
    answer = 0
    visited = (n+1) * [0]
    vertex = [[0] * (n+1) for _ in range (n+1)]
    
    for x in edge :
        vertex[x[0]][x[1]] = 1
        vertex[x[1]][x[0]] = 1
    
    def bfs(node) :
        visited[node] = 1
        q = deque()
        q.append((node,0))
        prevcost = 0
        cnt = 0
        
        while q :
            curnode,cost = q.popleft()
            if prevcost == cost :
                cnt+=1
            else :
                cnt=1
            
            prevcost = cost
            
            for i in range (1,n+1) :
                if not visited[i] and vertex[curnode][i] == 1 :
                    q.append((i,cost+1))
                    visited[i] = 1
        return cnt 
    answer = bfs(1)
    return answer
    ```
    
* 통과한 코드
``` python
from collections import deque
def solution(n, edge):
    answer = 0
    visited = (n+1) * [0]
    
    vertex = {i:[] for i in range (1,n+1)} # 인접 행렬을 표시하는 딕셔너리
    dists = {i:0 for i in range(1, n+1)} # 노드 1과 다른 노드들 사이의 거리

    for x in edge :
        vertex[x[0]].append(x[1])
        vertex[x[1]].append(x[0])
    
    def bfs(node) :
        visited[node] = 1
        q = deque()
        q.append((node,0))
        while q :
            curnode,dist = q.popleft()
            dists[curnode] = dist
            
            for x in vertex[curnode] : # 현재 노드에 연결되어 있는 모든 노드에 대해 탐색
                if not visited[x] :
                    q.append((x,dist+1))
                    visited[x] = 1 
    bfs(1) # bfs 수행
    
    maxdist = max(dists.values())
    for x in dists.values() :
        if x == maxdist :
            answer+=1
    return answer
```

코드의 차이점은 다음과 같다.
통과한 코드는, **인접행렬을 표현하는 자료구조가 리스트가 아니라 딕셔너리이다.** 
노드의 수가 많아지면 노드 끼리의 양방향 연결 상태를 나타내기 위해 리스트의 경우 n * n의 공간을 모두 할당하지만 딕셔너리의 경우 해당 노드에 연결된 노드들만 리스트에 담아주면 되어 공간복잡도를 줄일 수 있다. 그리고 해시를 기반으로 한 딕셔너리는 탐색시 시간 복잡도가 상수 시간으로 빠르기 때문에 문제를 통과할 수 있었다. 비슷한 문제를 풀때는 무조건 딕셔너리로 접근해야겠다!! 


출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges
