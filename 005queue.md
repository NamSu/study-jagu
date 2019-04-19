## 05. 큐

#### 5.1 큐란?

이전 단원에서는 스택이라는 자료구조 형태를 알아봤다. 큐는 스택과 비슷한 면이 있으면서 약간 다른 점이 존재한다.

#### 5.2 큐의 구조

큐는 **FIFO(First in First out)** 구조이다. 쉽게 말하면 선입선출인데, 처음 들어온 데이터가 처음으로 나가는 것이다. 그래서 아래와 같은 기본적인 함수들이 필요하다.

- full & empty
    - 다 찼거나 비었는지 체크하는 함수로, boolean 으로 return하면 편하다.
- enqueue
    - 큐의 **끝 부분에 데이터를 추가**하는 함수이다. if문으로 **full 상태**인지 판단한다.
- dequeue
    - 큐의 **맨 앞에 있는 데이터를 큐에서 제거하고 반환**하는 함수이다. if문으로 **empty 상태**인지 판단한다.
- peek
    - dequeue와 비슷하나, peek는 오직 큐의 **맨 앞에 있는 데이터만 읽어 반환**한다.

#### 5.3 선형 큐

선형 큐는 **1차원 배열을 사용하여 큐를 구현**하는 것으로, 기본적인 큐의 구조에 front, rear 변수가 추가된다.

아래와 같이 구현 가능하다.

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX_SIZE 5
typedef int element;
typedef struct {
    int front, rear;
    element data[MAX_SIZE];
} linear_queue;

void init_queue(linear_queue *lq) { // 큐 초기화
    lq->rear = -1; // 선형 큐는 -1
    lq->front = -1; // 선형 큐는 -1
}
void queue_print(linear_queue *lq) { // 큐 출력
    for (int i = 0; i < MAX_SIZE; i++) {
        if (i <= lq->front || i > lq->rear) { // 당연히 0보다 크거나 같아야 출력가능..
            printf(" ");
        } else {
            printf("%d _", lq->data[i]);
        }
    }
}
int check_full(linear_queue *lq) {
    if (lq->rear == (MAX_SIZE - 1)) { // rear가 최대 사이즈보다 작아야지 정상
        return 1;
    } else {
        return 0;
    }
}
int check_empty(linear_queue *lq) {
    if (lq->front == lq->rear) { // 숫자가 같으면 비어있는거.
        return 1;
    } else {
        return 0;
    }
}
void enqueue(linear_queue *lq, int queueitem) { // 큐에 데이터 삽입
    if (check_full(lq)) {
        fprintf("queue is full");
        return;
    } else {
        lq->data[++(lq->rear)] = queueitem;
    }
}
int dequeue(linear_queue *lq) { // 큐에서 제거 후 추출
    int dequeueitem;
    if (check_empty(lq)) {
        fprintf("queue is empty");
        return -1;
    } else {
        dequeueitem = lq->data[++(lq->front)];
    }
    return dequeueitem;
}
int peek(linear_queue *lq) { // 큐에서 추출만
    int peekitem;
    if (check_empty(lq)) {
        fprintf("queue is empty");
        return -1;
    } else {
        peekitem = lq->data[(lq->front)];
    }
    return peekitem;
}
int main(void) {
    ...
}
```

이렇게 간단하게 데이터 삽입/추출/출력이 가능한 선형 큐를 만들 수 있다.

#### 5.4 원형 큐

선형 큐는 **1차원 배열을 사용하여 큐를 구현**하는 것으로, 기본적인 큐의 구조에 front, rear 변수가 추가되며, **초기화 값이 -1 이 아닌 0이다.**

아래와 같이 구현 가능하다.