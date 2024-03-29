---
layout: post
title: 프로세스 동기화(Synchronization), 상호배제(Mutual Exclusion)
categories: OS
tags: [OS, CS]
---
### 임계구역(Critical Section)
- 공유 데이터가 있는 곳
- 데이터의 일관성을 위하여 둘 이상의 프로세스(쓰레드)가 동시에 접근해서는 안되는 곳
- 하나의 프로세스(쓰레드)가 공유 자원의 배타적인 사용을 보장받아야 함

* 임계구역 문제를 해결하기 위한 3가지 조건  
1. 상호배제 : 하나의 자원을 한 순간에 하나의 프로세스만 사용할 수 있도록 보장해야 한다.
2. 진행 : 임계구역에 실행중인 프로세스가 없다면, 어느 프로세스가 원할 때 사용하게 해줘야 한다.
3. 한정 대기 : 기아상태를 방지하기 위해 한번 들어갔다 나온 프로세스는 다음에 들어갈 때 제한을 준다.(횟수제한)

### 상호배제(Mutual Exclusion)
- 하나의 자원을 한 순간에 하나의 프로세스(쓰레드)만 사용할 수 있도록 보장하는 것
- 동기화가 없다면 데이터의 일관성을 위배할 수 있음
- ex) 은행계좌에 동시에 접근하면 인출이 두번 발생 할 수 있음.

### 경쟁조건(Race Condition)
- 공유 데이터를 둘 이상의 프로세스(쓰레드)가 요구하는 상황


## 동기화(상호배제) 알고리즘
### 뮤텍스(Mutex) : Binary 세마포어(0, 1)
#### (1) Dekker 알고리즘
- flag와 turn 변수를 통해 임계 구역에 들어갈 프로세스(스레드)를 결정
- flag : 프로세스 중 누가 임계영역에 진입할 것인지 나타내는 변수
- turn : 누가 임계구역에 들어갈 차례인지 나타내는 변수

```c
while(true) {
    flag[i] = true; // 프로세스 i가 임계 구역 진입 시도
    while(flag[j]) { // 프로세스 j가 현재 임계 구역에 있는지 확인
        if(turn == j) { // j가 임계 구역 사용 중이면
            flag[i] = false; // 프로세스 i 진입 취소
            while(turn == j); // turn이 j에서 변경될 때까지 대기
            flag[i] = true; // j turn이 끝나면 다시 진입 시도
        }
    }
}

------- 임계 구역 ---------

turn = j; // 임계 구역 사용 끝나면 turn을 넘김
flag[i] = false; // flag 값을 false로 바꿔 임계 구역 사용 완료를 알림
```

#### (2) Peterson 알고리즘  
- 데커와 유사하지만, 상대 프로세스(스레드)에게 진입 기회를 양보하는 것에 차이가 있음
- Busy Waiting을 하기 때문에 대기하면서 자원을 계속 사용해야 한다
- 변수
    - flag[i] : i번 프로세스가 임계구역 진입 의사를 표시하는 boolean 변수
    - turn : 임계구역에 배정된 프로세스  
  
```c
while(true) {
    flag[i] = true; // 프로세스 i가 임계 구역 진입 시도
    turn = j;   // 다른 프로세스에게 진입 기회 양보
    while(flag[j] && turn == j);    // 다른 프로세스가 진입 시도하면 대기
    ------- 임계 구역 ---------
    flag[i] = false;    // flag 값을 false로 바꿔 임계 구역 사용 완료를 알림
}
```

#### (3) Bakery 알고리즘
- 여러 프로세스/스레드에 대한 처리가 가능한 알고리즘. 가장 작은 수의 번호표를 가지고 있는 프로세스가 임계 구역에 진입

```c
while(true) {
    
    isReady[i] = true; // 번호표 받을 준비
    number[i] = max(number[0~n-1]) + 1; // 현재 실행 중인 프로세스 중에 가장 큰 번호 배정 
    isReady[i] = false; // 번호표 수령 완료
    
    for(j = 0; j < n; j++) { // 모든 프로세스 번호표 비교
        while(isReady[j]); // 비교 프로세스가 번호표 받을 때까지 대기
        while(number[j] && number[j] < number[i] && j < i);
        
        // 프로세스 j가 번호표 가지고 있어야 함
        // 프로세스 j의 번호표 < 프로세스 i의 번호표
    }
}

------- 임계 구역 ---------

number[i] = 0; // 임계 구역 사용 종료
```

### Counting 세마포어
#### (1) P, V 세마포어 알고리즘  
- 세마포어 : 2개의 원자적 함수(P, V)로 조작되는 정적 Integer 변수(S)
- P(S) : 임계 구역에 들어가기 전에 수행, S--, S<=0일때 프로세스를 슬립시킴 [P:try]
- V(S) : 임계 구역에서 나온 후에 수행, S++, 슬립중이 프로세스가 존재할 때 슬립된 프로세스 하나를 깨움 [V:increase]

```c
P(S) {
    S--;
    if (S < 0) {
        block()         // 이 프로세스를 슬립 큐에 추가
}

--- 임계 구역 ---

V(S) {
    S++;
    if (S <= 0) {
        wakeUp(process)  // 슬립 큐의 프로세스 하나를 깨움
    }
}
```



#### (2) 생산자-소비자 알고리즘 (= 유한버퍼 문제)
- 버퍼 : 유한한 아이템(데이터)를 보관하는 보관함 (공유 메모리)
- 생산자 : 아이템을 하나 만들면 버퍼에 저장, 버퍼가 꽉 차있을 때 자리가 날때 까지 계속 기다림
- 소비자 : 아이템이 필요할 떄 버퍼에서 아이템을 하나 가져옴, 버퍼가 비어있을 때 아이템이 하나 들어 올 때 까지 기다림

* 변수
  - empty : 버퍼 내의 빈 공간 (초기값 : n)
  - number : 버퍼 내의 저장된 아이템 수 
  - mutex : 버퍼 접근 통제

```c
producer() {
    while(true) {
        data = produce(); // 데이터 생산

        P(empty);          // 버퍼의 빈 공간 확인 후 접근
        P(mutex);          // 버퍼 사용

        append(data);     // 버퍼에 데이터 추가

        V(mutex);         // 버퍼 사용해제
        V(number);        // 버퍼에 데이터 추가 알림
    }
}

consumer() {
    while(true) {
        P(number);       // 버퍼에 데이터 존재 확인
        P(mutex);        // 버퍼 사용

        data = take();   // 버퍼에서 데이터 가져옴

        V(mutex);       // 버퍼 사용해제
        V(empty);      // 버퍼에 데이터 소모 알림

        consume(data);  // 데이터 처리
    }
}

```