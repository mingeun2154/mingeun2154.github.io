---
title: Dijkstra 최단경로 알고리즘 O(N²)
date: 2022-2-22 15:32
categories: [algorithm, 최단경로]
layout: post
toc: true
tags: [dijkstra]     # TAG names should always be lowercase
---
- - -
dijkstra 최단경로 알고리즘은 __하나의 시작노드에서부터 다른 모든 노드까지__ 의 최단경로를 구하는 알고리즘이다.   

## 시간 복잡도 : O(V²)
V는 노드(vertex)의 개수이다.    
알고리즘에 따라 달라질 수 있겠지만 일반적으로 괄호 안의 값이 10⁸(1억)일때 1초가 걸린다.

## 알고리즘 작동 방식
```python
for n-1번(시작 노드를 제외한 나머지 노드의 수) 반복:
	최단경로 리스트(distance)에서 값이 가장 작은 노드(now)를 뽑는다.   
	now를 방문 처리     
	for now에 인접한 모든 노드(graph[i[0]])에 대해 반복:     
		now에서 graph[i[0]]까지의 거리를 갱신    
```
n-1번 반복하는 각각의 단계에서 시간복잡도가 O(V)인 get_smallest_node()를 호출하기 때문에 전체 코드의 시간복잡도는 O(V²)이 된다.

## 코드

```python
import sys
input=sys.stdin.readline
INF=int(1e9)
# 노드,간선의 개수 입력
n,m=map(int, input().split())
# 시작 노드 입력
start=int(input())
# graph, distance, visited 초기화
graph=[[] for _ in range(n+1)]   
distance=[INF]*(n+1)    
visited=[False]*(n+1)   
# 모든 간선의 정보 입력
for _ in range(m):
	a,b,c=map(int, input().split())
	graph[a].append((b,c))

# 최단경로가 가장 짧은 노드의 번호를 반환하는 함수
def get_smallest_node():
	min_value=INF
	index=0
	# 반복문
	for i in range(1,n):
		# 방문하지 않았던 노드 i까지의 거리가 더 짧다면 min_value 갱신
		if distance[i]<min_value and not visited[i]:
			min_value=distance[i]
			index=i
	return index

# dijkstra 알고리즘
def dijkstra(start)
	# 시작 노드에 대해 초기화
	visited[start]=True
	distance[start]=0
	# 시작 노드를 제외한 나머지 노드들에 대해 n-1번 반복
	for _ in range(n):
		# 시작 노드로부터 최단거리가 가장 짧은 노드를 하나 선택
		now=get_smallest_node()
		visited[now]=True
		# 현재 노드(now)에서부터 인접 노드까지의 거리를 갱신
		for i in graph[now]:
			cost=distance[now]+i[1]
			if distance[i[0]]>cost:
				distance[i[0]]=cost

dijkstra(start)
# 결과 출력
for i in range(1,n+1):
	if distance[i]==INF:
		print("INFINITY")
	else:
		print(distance[i])
```

