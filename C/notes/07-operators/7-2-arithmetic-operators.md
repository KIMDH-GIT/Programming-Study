# 7-2. `+`, `-`, `*`, `/`, `%`

산술 연산은 promotion과 usual arithmetic conversions 뒤 공통형에서 수행된다.
## 1. 학습 목표
- 다섯 이항 산술 연산을 설명한다.
- 정수 몫과 나머지 관계를 적용한다.
- signed overflow와 unsigned wrap을 구별한다.
## 2. 선수 지식
Part 3 overflow와 Part 6 변환 규칙을 사용한다.
## 3. 핵심 개념
`+`, `-`, `*`, `/`는 산술형 피연산자에 적용된다. `%`는 정수형에만 적용된다. 정의된 정수 나눗셈에서 `(a / b) * b + a % b == a`다. 몫은 0 방향으로 절단되므로 나머지는 피제수 `a`와 같은 부호이거나 0이다. signed 결과가 표현 범위를 넘으면 UB, unsigned는 법에 따른다.
## 4. 문법
```c
int q = a / b;
int r = a % b;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int a = -17;
    int b = 5;
    printf("%d %d %d\n", a / b, a % b, (a / b) * b + a % b);
    return 0;
}
```
## 6. 코드 해석
몫은 -3, 나머지는 -2이며 복원식은 -17이다.
## 7. 내부 동작
컴파일러는 여러 CPU 명령으로 구현할 수 있다. C 값 규칙이 우선이다.
## 8. 자주 하는 실수
- 음수 몫을 내림으로 계산한다.
- 나머지는 항상 양수라고 생각한다.
- 모든 overflow가 wrap한다고 생각한다.
## 9. 필수 실습
안전한 양수·음수 나눗셈의 복원식을 확인한다. [실습 README](../../exercises/07-operators/7-2/README.md).
## 10. 추가 실습
- ★ 기초: 17과 5.
- ★★ 응용: -17과 5.
- ★★★ 도전: unsigned 경계 분석.
## 11. 확인 문제
1. `%`는 실수형에 사용할 수 있는가?
2. `-17/5`는?
3. `-17%5`는?
4. signed overflow의 분류는?
## 12. 핵심 정리
산술은 변환 뒤 공통형에서 수행되며 정수 몫·나머지는 복원 관계를 만족한다.
## 13. 다음 Step
[Step 7-3. 나눗셈 경계: 0과 `INT_MIN / -1`](7-3-division-boundaries.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5~6.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [GCC integer implementation](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
