---
layout: post
title: 알고리즘을 위한 파이썬
categories: Algorithm
tags: [Algorithm, CS, Python]
---

### 1. 리스트(List)
- 크기가 n이고, 모든 값이 0인 1차원 리스트 초기화

```python
n = 10
array = [0] * n

print(array)
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

- 크기가 n*m이고, 모든 값이 0인 2차원 리스트 초기화

```python
n = 3
m = 4
array = [[0] * m for _ in range(n)]

print(array)
[[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

- 리스트 Comprehension

```python
# 0~19 중 홀수만 포함하는 리스트
array = [i for i in range(20) if i % 2 == 1]

print(array)
[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
```

- 리스트 Method  
(1) append(v) : 리스트에 v를 삽입 / O(1)  
(2) insert(i, v) : i 위치에 v를 삽입 / O(N)  
(3) remove(v) : 리스트에서 v를 삭제, 여러개일 경우 하나만 삭제  

```python
array = [1, 4, 3]

array.append(2)
print(array)
[1, 4, 3, 2]

array.insert(2, 5)
print(array)
[1, 4, 5, 3, 2]

array.remove(4)
print(array)
[1, 5, 3, 2]
```

### 2. 입출력
- 각 데이터를 공백으로 구분하여 입력

```python
data = list(map(int, input().split()))
data.sort()

65 90 75 34 99
print(data)
[34, 65, 75, 90, 99]
```

- 한 줄씩 입력

```python
import sys

data = sys.stdin.readline().rstrip()
```


### 3. 라이브러리
#### (1) itertools : 반복, 순열/조합
```python
from itertools import permutations, combinations, product, combination_with_replacement

data = ['A', 'B', 'C']

# 1. 순열
per = list(permutations(data, 3))
print(per) # 6개
[('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]

# 2. 조합
com = list(combinations(data, 2))
print(com) # 3개
[('A', 'B'), ('A', 'C'), ('B', 'C')]

# 3. 중복순열
pro = list(product(data, repeat=2))
print(pro) # 9개
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'B'), ('B', 'C'), ('C', 'A'), ('C', 'B'), ('C', 'C')]

# 4. 중복조합
comr = list(combination_with_replacement(data, 2))
print(comr) # 6개
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]
```

#### (2) heapq : 힙(우선순위큐)
- 파이썬의 힙은 최소힙만 제공
- heappush(), heappop()

#### (3) bisect : 이진탐색
- bisect_left(a, x) : 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 왼쪽 인덱스를 찾음
- bisect_right(a, x) : 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 오른쪽 인덱스를 찾음

```python
from bisect import bisect_left, bisect_right

a = [1, 2, 4, 4, 8]
x = 4

print(bisect_left(a, x))  # 2
print(bisect_right(a, x))  # 4
```

#### (4) collections : 데크, 카운터
1\. 데크
- popleft() : 첫번째 원소 제거
- pop() : 마지막 원소 제거
- appendleft(x) : 첫번째 인덱스에 삽입
- append(x) : 마지막 인덱스에 삽입

```python
from Colletions import deque

data = deque([2, 3, 4])
data.appendleft(1)
data.append(5)

print(data)
[1, 2, 3, 4, 5]
```

2\. 카운터

```python
form collections import Counter

counter = Counter(['red', 'blue', 'red', 'green', 'blue', 'blue'])

print(counter(['blue'])
3
print(dict(counter))
{'red': 2, 'blue': 3, 'green': 1}
```


#### (5) math
- factorial(a) : 펙토리얼
- sqrt(a) : 제곱근
- gcd(a, b) : 최대공약수

```python
import math

math.factorial(5)   # 120
math.sqrt(7)        # 2.6457513110645907
math.gcd(21, 14)    # 7
```