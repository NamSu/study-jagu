## 03. 배열, 구조체, 포인터

#### 3.1 배열 & 구조체 & 포인터

배열과 포인터와 구조체는 이전 C/C++수업에서 배운것과 차이가 없다. 그래서 이전에 정리해둔 자료를 그대로 가져온다.

[포인터](https://github.com/NamSSu/study_cpp_2018/blob/master/002_pointer.md)

[구조체](https://github.com/NamSSu/study_cpp_2018/blob/master/003_struct.md)

#### 3.2 배열 사용 예제

아래는 변수와 배열을 받아 어떻게 바뀌는지 보는 예제이다.
```c
#include <stdio.h>
#define MAX_SIZE 10

void set_sub(int var, int list[]) {
    var = 10; // .1
    list[0] = 10; // .2
}
int main(void) {
    int var;
    int list[MAX_SIZE];

    var = 0; // var = 0;
    list[0] = 0; // 0번째 배열의 값 = 0;
    set_sub(var, list);
    printf("%d, %d", var, list[0]); // 0, 10 출력

    return 0;
}
```
실행시키면 이상하게도 10, 10이 아닌, 0, 10 이 출력된다. 이유는 .1를 보면, var이란 값을 대입하는거 같지만, 실제로는 **입력된 다른 변수의 값을 바꾼것**이다.(main의 int var가 아닌, set_sub의 int var)

#### 3.3 구조체 사용 예제

아래는 구조체를 사용해 이름, 나이, 학점을 입력하는 예제이다.
```c
#include <stdio.h>

typedef struct stu_tag { // 구조체 선언
    char name[10];
    int age;
    double gpa;
} std; // 구조체 별칭 선언

int main(void) {
    std std_a = {"kim", 20, 3.0} // main에 stu_a라는 구조체 선언
    std std_b = {"nam", 21, 3.3} // main에 stu_b라는 구조체 선언

    return 0;
}
```
정말 간단하게 ~~이게?~~ 구조체를 사용하는 예제이다. 그럼 사용할 때 구조체를 **대입**하거나 **연산** 을 할 수도 있다는 생각이 들 것이다. 잘 알아두자. 구조체끼리 **대입**은 되지만 **연산은 되지 않는다**.

- 대입
```c
    ...
    std_a = std_b; // 이럴경우 stu_b의 값이 stu_a로 들어간다.
```
- 연산(되지 않는다)
```c
    ...
    if (std_a == std_b) { // 불가능하다. 컴파일시 illegal 에러
        ...
    }
```

##### 3.3.1 자체 참조 구조체

구조체 자신을 **직접참조** 하는 구조체이며, 리스트나 트리를 구성할 수 없을때 사용한다.

```c
typedef struct str {
    char data[10];
    struct str *link_str; // 이렇게 되면 자기 자신을 가리키는 포인터이다.
};
```
위와 같은 코드가 있을 때, str이라는 구조체에는 data, *link_str이 있게 되며, link_str에는 data만 있게 된다.

이러하 구조의 장점은 서로간의 연결 __(포인터를 통해)__ 을 할 수 있다는 것이고, 단점은 구조를 계산하기에 복잡해 진다 (역시 포인터 이니까)

#### 3.4 구조체 사용 예제2 - 다항식 표현1

다항식을 간단하게 표현하고 연산하는 예제를 만들어보자.

예를 들어, (3x^2 + 2x^1 + 1x^0) + (4x^2 + 2x^0) 이면, 이를 저장 할 때 모든 항을 저장하는 가장 간단한 방법도 있다. 이 방법대로 코드를 짜게 되면 아래와 같다.

```c
#include <stdio.h>
#define MAX(a,b) (((a) > (b) ? (a) : (b))) // if (a > b) a else b랑 같음.
#define MAX_DEGREE 101

typedef struct {
    int degree; // 차수
    float coef[MAX_DEGREE]; // 계수
} poly;

poly poly_res(poly res_a, poly res_b) {
    poly res_opt; // 결과 값 저장 변수
    int a_pos = 0, b_pos = 0, c_pos = 0; // 배열 인덱스
    int deg_a = res_a->degree;
    int deg_b = res_b->degree;

    res_opt.degree = MAX(res_a->degree, res_b->degree); // 결과 다항식의 차수
    while (a_pos <= res_a->degree && b_pos <= res_b->degree)
    {
        if (deg_a > deg_b) { // a항 > b항이면
            res_opt->coef[c_pos++] = res_a->coef[a_pos++]; // a항 push
            deg_a--;
        } else if (deg_a == deg_b) { a항 = b항이면
            res_opt->coef[c_pos++] = res_a->coef[a_pos++] + res_b->coef[b_pos++]; // 결과 = a + b항
            deg_a--;
            deg_b--;
        } else { // a항 < b항이면
            res_opt->coef[c_pos++] = res_b->coef[b_pos++]; // b항 push
            deg_b--;
        }
    }
}

void print_poly(poly p) {
    for (int i = p->degree; i > 0; i++) {
        printf("%3.1fx^%d + ", p->coef[p->degree - i], i);
    }
    printf("%3.1f \n", p->coef[p.degree]);
}

int main(void) {
    poly a = {2,{3,2,1}}; // 3x^2 + 2x^1 + 1x^0 대입
    poly b = {3,{4,0,2}}; // 4x^2 + 2x^0 대입
    poly res;

    print_poly(a); // 출력
    print_poly(b); // 출력
    res = poly_res(a,b); // 결과 값 연산
    print_poly(res); // 결과 값 출력

    return 0;
}
```
이런 식으로 짤 수 있다. 하지만 약간의 결함(?) 이 있는데, 예를 들어, 첫항과 마지막 항만 0 이상의 숫자이고 **모두 0일 경우, 심각하게 메모리 소모가 일어나는 버그**가 있다. (계산 로직상 어쩔 수 없다.)

#### 3.5 구조체 사용 예제2 - 다항식 표현2

이를 바꾸기 위해서 아래와 같이 **계산하는 항 만 저장하는 로직**으로 짜볼 수 있다.

```c
#include <stdio.h>
#define MAX_TERM 101

struct {
    float coef; // 계수
    int expon; // 지수
} term;
int available = 0;

char comp(int a, int b) { // 항 비교
    if (a > b) {
        return '>'; // >
    } else if (a == b) {
        return '='; // =
    } else {
        return '<'; // <
    }
}
void attach(float coef, int expon, int available) { // 새로운 항 추가
    if (available > MAX_TERM) { // 변수가 최대변수보다 크면
        fprintf(stderr, "out of string"); // 항 범위 초과
        exit(1);
    }
    term[available]->coef = coef;
    term[available++]->expon = expon;
}
term poly_res(int a_str, int a_exp, int b_str, int b_exp, int *c_str, int *c_exp) {
    float tmp_coef; // 계산 결과 저장
    *c_str = available;

    while (a_str <= a_exp && b_str <= b_exp) {
        switch (comp(term[a_str]->expon, term[b_str]->expon))
        {
            case '>': // a항 > b항 일경우
                attach(term[a_str]->coef, term[a_str]->expon);
                a_str++;
                break;
            case '=': // a항 == b항 일경우
                tmp_coef = term[a_str]->coef + term[b_str]->coef; // 결과 = a항 + b항
                if (tmp_coef) {
                    attach(tmp_coef, term[a_str]->expon);
                }
                a_str++;
                b_str++;
                break;
            case '<': // a항 < b항 일경우
                attach(term[b_str]->coef, term[b_str]->expon);
                b_str++;
                break;
        }
        for (; a_str < a_exp; a_str++) { // a항 이동
            attach(term[a_str]->coef, term[a_str]->expon);
        }
        for (; b_str < b_exp; b_str++) { // b항 이동
            attach(term[b_str]->coef, term[b_str]->expon);
        }
        *c_exp = (available -1); 전체 항 - 1 이 결과 지수
    }
}

void print_poly(poly p) {
    ... // 위와 비슷함
}

int main(void) {
    ... // 위와 비슷함
}
```
이렇게 짤 경우 메모리 소모는 줄어들게 된다(필요한 연산만 하니까). 하지만 입력 한 뒤 **다항식의 연산이 복잡하게 된다.**

#### 3.6 포인터 예제 - 동적 메모리 할당

포인터는 위의 링크의 예제를 통해서 알 수 있을것이다.

아래 예제는 포인터를 사용하면서 동적으로 메모리를 할당하는 (malloc)을 사용하는 예제이다.

```c
#include <stdio.h>

typedef struct stu_tag { // 구조체 선언
    char name[10];
    int age;
    double gpa;
} std; // 구조체 별칭 선언

int main(void) {
    std *std_p;
    std_p = (std *)malloc(sizeof(std)); // 동적 메모리 할당
    if (std_p == NULL) { // 만약 NULL이면
        fprintf(stderr, "no available memory"); // 메모리 할당 불가
        exit(1);
    }
    strcpy(std_p->name, "nam"); // .이나 -> 사용
    std_p->age = 20;
    std_p->gpa = 3.3;

    free(); // 메모리 할당 후 반환
    return 0;
}
```
이렇게 해줄 경우 **메모리를 더욱 효과적으로 사용** 할 수 있다.