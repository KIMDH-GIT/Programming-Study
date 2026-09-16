# 9-4. `for`

`for`는 초기화, 조건, 반복 식을 한 머리 부분에 모아 횟수 중심 반복을 표현한다.

## 1. 학습 목표
- `for`의 네 구성 요소를 구분한다.
- 정확한 평가 순서를 추적한다.
- C17의 초기화 선언 범위를 설명한다.

## 2. 선수 지식
Step 9-1의 반복 요소와 Step 9-2의 선평가 흐름을 사용한다.

## 3. 핵심 개념
`for`에는 **initialization clause**, **condition clause**, **iteration expression**, **body**가 있다. 초기화는 전체 반복에서 한 번, 조건은 각 본문 전에, 반복 식은 각 정상적인 본문 실행 뒤에 평가된다. C17에서는 초기화 절에 `int i = 0` 같은 선언을 사용할 수 있고 이때 `i`의 scope는 `for` 문 전체다.

## 4. 문법
```c
for (initialization; condition; iteration) {
    body;
}
```
세 절은 생략할 수 있지만 두 세미콜론은 남는다. `for (;;) { }`는 조건에 의해 종료되지 않는 반복이다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int i = 0; i < 5; ++i) {
        printf("%d\n", i);
    }
    return 0;
}
```

## 6. 코드 해석
`i`를 0으로 한 번 초기화한다. `i < 5`가 참이면 출력하고 `++i`를 평가한 뒤 다시 검사한다. 0~4가 출력되며 `i == 5`인 검사는 거짓이다.

| iteration | 조건 전 `i` | `i < 5` | body | update 후 |
|---:|---:|:---:|---|---:|
| 1 | 0 | 참 | 0 출력 | 1 |
| 2 | 1 | 참 | 1 출력 | 2 |
| 3~5 | 2~4 | 참 | 값 출력 | 3~5 |
| 종료 | 5 | 거짓 | 실행 안 함 | - |

## 7. 내부 동작
[C 언어 관점] `continue`는 iteration expression으로 이동한 뒤 조건을 다시 검사한다. 조건 절을 생략하면 0이 아닌 상수처럼 계속하는 것으로 취급한다. signed `i`가 범위를 넘는 반복은 안전한 무한 반복이 아니며 overflow는 undefined behavior다.

## 8. 자주 하는 실수
- 본문 다음에 조건을 바로 검사하고 iteration expression을 빼먹는다.
- 조건에서 `i <= 5`를 써서 6회 실행한다.
- `for` 머리 뒤에 실수로 `;`를 써 빈 statement만 반복한다.
- `i++ + ++i`처럼 한 full expression에서 같은 객체를 unsequenced하게 여러 번 변경한다.

## 9. 필수 실습
0부터 4까지 출력하며 각 절의 평가 횟수를 기록한다. [실습 README](../../exercises/09-loops/9-4/README.md)

## 10. 추가 실습
- ★ 1부터 5까지
- ★★ 0부터 10까지 2씩 증가
- ★★★ `while`로 같은 흐름을 작성해 대응 요소 표시

## 11. 확인 문제
1. initialization clause는 몇 번 실행되는가?
2. condition이 처음 거짓이면 body는 몇 번 실행되는가?
3. `continue` 뒤에는 어디로 이동하는가?
4. `for (;;)`가 조건에 의해 끝나지 않는 이유는?
5. 초기화에서 선언한 `i`의 scope는 어디까지인가?

## 12. 핵심 정리
`for`의 순서는 초기화 1회 → 조건 → 본문 → 반복 식 → 조건이며, 횟수와 변화가 한눈에 보인다.

## 13. 다음 Step
[Step 9-5. `break`와 `continue`](9-5-break-continue.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5.3
- [cppreference: for loop](https://en.cppreference.com/w/c/language/for.html)
