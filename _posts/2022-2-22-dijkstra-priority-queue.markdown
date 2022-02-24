---
title: Dijkstra 최단경로 알고리즘 O(E㏒V)
date: 2022-2-22 16:53
categories: [algorithm, 최단경로]
layout: post
toc: true
tags: [dijkstra, priority queue]     # TAG names should always be lowercase
---
- - -

__우선순위 큐(priority queue)__를 이용하여 dijkstra 최단경로 알고리즘을 구현할 수 있다

## 시간 복잡도 : O(E㏒V)
> E는 간선(edge)의 개수, V는 정점(vertex)의 개수이다. 시간복잡도에서 log의 밑(logarithm base)는 2이다.    

이 알고리즘에서는 시간 복잡도가 O(V)인 get_smallest_node() 대신 시간복잡도가 O(㏒)인 우선순위 큐를 사용한다.

## 알고리즘 작동 방식
```python
while 큐가 빌때까지 반복:
	큐에서 최단경로 값이 가장 작은 노드를 꺼낸다
	꺼낸 노드가 이미 계산된 노드인지 확인
		처리되었다면 continue
	for 현재 노드의 모든 인접노드에 대해 반복
		현재노드에서부터 인접노드까지의 거리 갱신
		갱신된 인접노드는 큐에 삽입
```

## 코드
```python
import sys
import heapq
input=sys.stdin.readline
INF=int(1e9)
# 노드, 간선의 개수 입력
n, m=map(int, input().split())
# 시작 노드 입력
start=int(input())
# graph, distance 초기화
graph=[[] for _ in range(n+1)]
distance=[INF]*(n+1)
# 모든 간선의 정보 입력
for _ in range(m):
	a,b,c=map(int, input().split())
	graph[a].append((b,c))
# 알고리즘
def dijkstra(start):
	q=[]
	# 시작노드에 대해 초기화
	distance[start]=0
	for i in graph[start]:
		distance[i[0]]=i[1]
	# 우선순위 큐에 ( 시작노드에서부터 집어넣는 노드까지의 거리, 노드 번호) 튜플 삽입
	heapq.heappush(q, (0, start))
	# 큐가 비어있지 않은 동안 반복
	for q:
		# 큐에서 최단거리 값이 가장 작은 노드 하나를 꺼낸다.
		dist, now=heapq.heappop(q)
		# 뽑은 노드가 이미 처리된 노드인지 확인
		if distance[now]<dist:
			continue
		# now에서부터 인접 노드까지의 거리 갱신
		for i in graph[now]:
			cost=distance[now]+i[1]
			if distance[i[0]]>cost:
				distance[i[0]]=cost
				heapq.heappush(q, (cost, i[0]))
	
	dijkstra(start)
	# 결과 출력
	for i in range(1, n+1):
		if distance[i]==INF:
			print("INFINITY")
		else:
			print(distance[i])
```

