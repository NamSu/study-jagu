## 04. 스택

#### 4.1 스택이란?

지금까지 자료구조를 공부하면서 **배열**을 활용하는 예제를 많이 봤을 것이다. 사용하면서 **"어떻게 집어넣고, 빼는거지?"** 라는 생각도 어느정도는(?) 해 봤을 것이다. 그래서 저장하는 자료구조인 **스택**에 대해서 알아볼 것이다.

#### 4.2 스택의 구조

스택은 **LIFO(Last in First Out)** 구조이다. 쉽게 말하면 후입선출인데, 가장 마지막에 들어온 데이터가 먼저 나가는 것이다. 그래서 아래와 같은 기본적인 함수가 있어야 한다.

- top
    - 상단을 표현하는 함수로, push와 pop을 진행 할 때 필요하다.
- push
    - 데이터를 집어넣는 함수로, stack이 비어있는지 체크 한 뒤에 데이터를 집어넣는다.
- pop
    - 데이터를 꺼내는 함수로, stack이 비어있는지 체크 한 뒤에 데이터를 꺼낸다.
- full & empty
    - 다 찼거나 비었는지 체크하는 함수로, boolean 으로 return하면 편하다.
- peek
    - pop과 비슷한 역활을 하는 함수이다. 여기서 **pop과 비슷한데 왜 쓰는지**에 대한 의문이 있을수도 있는데, **pop은 데이터를 가져오며 stack에서 삭제하고**, **peek는 데이터를 가져오기만 한다.**

#### 4.3 스택의 구현 - 1

아래와 같이 기초적인 스택을 구현 가능하다.

스택 크기는 100 고정이다.
```c
#include <stdio.h>
#include <stdlib.h>

typedef int element;
element stack[100]; // element 구현
int top = -1; // 최초 값 -1

int check_empty() {
    return (top == -1); // top이 -1일경우 return
}
int check_full() {
    return (top == (100 - 1)); // top이 99일경우 return
}
void push(element stackitem) {
    if (check_full()) { // stack이 다 찼을경우
        fprintf("stack is full");
        return;
    } else {
        stack[++top] = stackitem; // 대입
    }
}
element pop() {
    if (check_empty()) {
        fprintf("stack is empty");
        exit(1);
    } else {
        return stack[top--]; // 삭제 후 출력
    }
}
element peek() {
    if (check_empty()) {
        fprintf("stack is empty");
        exit(1);
    } else {
        return stack[top]; // 그냥 출력
    }
}

int main(void) {
    ...
}
```

이렇게 기초적인 방법으로 스택을 구현 가능하다. 하지만 가변적인 스택을 생성 불가하며, 재사용이 불가능하다. 그래서 아래와 같이 **구조체와 동적 메모리 할당을 이용**해 더욱 완벽한 스택을 만들 수 있다.

#### 4.4 스택의 구현 - 2

위에서 예시를 든 것처럼, **구조체와 동적 메모리 할당**을 이용해서 예제를 만들었다.

```c
#include <stdio.h>
#include <stdlib.h>
typedef int element;
typedef struct {
    element *data;
    int capacity;
    int top;
} stacktype;

int check_empty(stacktype *s) {
    return (s->top == -1); // top이 -1일경우 return
}
int check_full(stacktype *s) {
    return (s->top == (100 - 1)); // top이 99일경우 return
}
void stack_init(stacktype *s) {
    s->top = -1;
    s->capacity = 1;
    s->data = (element *)malloc(s->capacity * sizeof(element)); // 메모리 할당
}
void stack_del(stacktype *s) { // 스택 제거
    free(s);
}
void push(stacktype *s element stackitem) {
    if (check_full(s)) {
        s->capacity += 1; // 용량 부족시 +1씩 증가
        s->data = (element *)realloc(s->data, s->capacity * sizeof(element)); // 메모리 제할당
    }
    s->data[++(s->top)] = stackitem;
}
element pop(stacktype *s) {
    if (check_empty(s)) {
        fprintf("stack is empty");
        exit(1);
    } else {
        return s->stack[(s->top)--]; // 삭제 후 출력
    }
}
element peek(stacktype *s) {
    if (check_empty(s)) {
        fprintf("stack is empty");
        exit(1);
    } else {
        return s->stack[(s->top)]; // 그냥 출력
    }
}

int main(void) {
    ...
}
```