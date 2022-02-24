---
title: Floyd Warshall 최단경로 알고리즘
date: 2022-2-22 17:41
categories: [algorithm, 최단경로]
layout: post
toc: true
tags: [floyd warhall]     # TAG names should always be lowercase
---
- - -

Floyd Warshall 최단경로 알고리즘은 **모든 노드에서부터 모든 노드까지의**  최단경로를 계산한다.

## 시간 복잡도 : O(V³)
노드의 개수가 10³보다 커지면 연산 속도가 매우 느려진다(10초 이상)

## 알고리즘 작동 방식

graph[i][j]=c : i번 노드에서 j번 노드까지의 거리가 c 라는 의미이다.
     
<img src="/assets/img/shortest_path/graph.jpeg" width="60%" height="60%" alt="graph">


```python
for k가 1부터 n까지 반복:
	for i가 1부터 n까지 반복:
		for j가 1부터 n까지 반복:
			i노드에서 j노드까지의 최단거리는 min(i에서 j까지의 거리, i에서 k를 거쳐 j까지 가는 거리)
```

실선과 점선으로 표현된 경로의 비용을 비교하여 더 작은 값을 선택한다.
   
<img src="/assets/img/shortest_path/diagram.jpeg" width="60%" height="60%" alt="diagram">

## 코드
```python
import sys
input=sys.readline.input
INF=int(1e9)
# 노드, 간선 개수 입력
n,m=map(int, input().split())
# graph 초기화
graph=[[INF]*(n+1) for _ in range(n+1)]
for i in range(1, n+1):
	graph[i][i]=0
# 모든 간선 정보 입력
for _ in range(m):
	a,b,c=map(int,input().split())
	graph[a][b]=c

# 알고리즘
for k in range(1, n+1):
	for i in range(1, n+1):
		for j in range(1, n+1):
			graph[i][j]=min(graph[i][k]+graph[k][j], graph[i][j])

# 결과 출력
for i in range(1,n+1):
	for j in range(1,n+1):
		if graph[i][j]==INF:
			print("INFINITY", end=' ')
		else:
			print(graph[i][j], end=' ')
	print()
```
