## 02. 순환과 반복

#### 2.1 순환이란

**순환** 이란 알고리즘이나 함수가 수행 도중에 자기 자신을 호출 ~~(재귀)~~ 하여서 문제를 해결하는 기법이다.

예를 들어,
- 팩토리얼 값 구하기
- 피보나치 수열
- 이항계수
- 이진탐색

##### 2.1.1 순환 예제

예를 들어, 팩토리얼 예제를 보면,
```c
int factorial (int number) {
    ...
    if (number <= 1) {
        return 1; // 순환 호출을 멈추는 부분
    } else {
        return (number * factorial(number - 1)); // 순환 호출을 하는 부분
    }
}
```
들어온 숫자가 1보다 작거나 같을 경우 1을 리턴하고, 아닐경우 (입력 된 숫자 * 팩토리얼 함수에 입력된 숫자에서 -1 을 한 값)을 리턴한다.

이러한 순환 코드를 **순환 알고리즘** 이라고 한다. 이 알고리즘은 다음과 같은 부분을 포함하는데,

- 순환 호출을 하는 부분
- 순환 호출을 멈추는 부분

으로 나뉘게 된다.

### 2.2 순환 vs 반복

잠시만 생각해보자, 순환은 결국 반복을 한다는 의미인데 굳이 둘을 나눌 필요가 있을까라는 생각이 들 것이다.

같은 점은
- 순환은 **반복과 상호 호환적 관계**이다.

다른 점은
##### 순환
- 무언가 **순환**적인 문제에서는 반복보다 자연스럽다 ~~사람이 느끼기에~~
- 함수를 호출하는 (CallBy~) 오버헤드이다.
##### 반복
- 수행 속도가 순환에 비해 **빠르다**
    - 생각해보자. 일련 된 과정을 수행하는데 호출 과정이 없으니 더 빠를 것이다.
- 단, 정말 **순환**적인 문제에 대해서는 순환문제를 반복문제로 전환 하는 과정이 매우 복잡할 수도 있다.
    - 생각해보자. 팩토리얼 문제를 반복만으로 풀 수도 있지만, 이진 탐색 문제같은 복잡한 과정을 반복만으로 풀기에는 복잡 할 것이다.