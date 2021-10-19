---
layout: post
title: 백트래킹 알고리즘(Backtracking)
categories: Algorithm
tags: [Algorithm, CS]
---

- 해를 찾는 도중에 지금 경로에서 해를 못 구할거 같으면 더이상 가지 않고 되돌아가 다른 경로를 선택해 해를 찾아가는 알고리즘
- DFS(깊이 우선 탐색)을 기반으로 하는 알고리즘
- 가지치기를 통해 시간효율을 높여준다
  - 가지치기(Pruning) : 해를 찾는 도중에 해가 아닐시 바로 중단하여 진행하지 않는 것
- 최적화 문제와 결정 문제를 푸는데 사용된다
  - 결정문제 : 해가 존재하는지 여부를 Yes or No 로 답하는 문제 

- DFS vs 백트래킹
  - DFS : 반드시 모든 노드를 거친다
  - 백트래킹 : 가능성이 있는 노드만 거친다


### 문제
#### 1. N-Queen 문제