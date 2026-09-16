# 7-6. `=`, `+=`, `-=`, `*=`, `/=`, `%=`

대입은 이미 존재하는 객체에 값을 저장한다. 초기화와 구별하고 복합 대입의 단일 객체 평가 의미를 이해한다.
## 1. 학습 목표
- initialization과 assignment를 구별한다.
- 여섯 대입 연산을 설명한다.
- 복합 대입의 변환과 경계를 설명한다.
## 2. 선수 지식
객체, lvalue, 산술 변환을 사용한다.
## 3. 핵심 개념
`int x = 10;`은 초기화, `x = 20;`은 대입이다. `x += y`는 대략 `x = x + y`와 같은 값 계산을 하지만 왼쪽 피연산자를 한 번만 평가한다. 저장 전에는 왼쪽 객체형으로 변환된다. `/=`, `%=`에도 0 제수 경계가 있다.
## 4. 문법
```c
value = 10;
value += 3;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int value = 10;
    value += 5;
    value *= 2;
    value -= 4;
    value /= 2;
    value %= 7;
    printf("%d\n", value);
    return 0;
}
```
## 6. 코드 해석
값은 10→15→30→26→13→6으로 바뀐다. 모든 제수는 0이 아니다.
## 7. 내부 동작
대입 식도 값을 가지지만 이번에는 statement로만 사용한다. 실제 저장 명령 수는 최적화에 따라 달라질 수 있다.
## 8. 자주 하는 실수
- 선언 초기화를 대입 연산으로만 설명한다.
- 복합 대입에 변환이 없다고 생각한다.
- `/= 0`을 실행한다.
## 9. 필수 실습
한 객체에 복합 대입을 순서대로 적용한다. [실습 README](../../exercises/07-operators/7-6/README.md).
## 10. 추가 실습
- ★ 기초: `+=`, `-=`.
- ★★ 응용: `*=`, `/=`.
- ★★★ 도전: 저장 전 변환 분석.
## 11. 확인 문제
1. 초기화와 대입의 차이는?
2. `x += y`의 왼쪽은 몇 번 평가되는가?
3. 대입 전 어떤 변환이 있는가?
4. `/=`의 UB 경계는?
## 12. 핵심 정리
대입은 기존 객체를 변경하며 복합 대입도 산술·변환·경계 규칙을 따른다.
## 13. 다음 Step
[Step 7-7. 전위·후위 `++`, `--`](7-7-increment-decrement.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.16.
- [cppreference: assignment operators](https://en.cppreference.com/w/c/language/operator_assignment.html)
- [cppreference: conversion](https://en.cppreference.com/w/c/language/conversion.html)
