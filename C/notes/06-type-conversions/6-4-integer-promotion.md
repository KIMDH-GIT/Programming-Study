# 6-4. integer promotion

`char`, `short`, `_Bool`과 작은 열거형 값은 많은 식에서 먼저 `int` 또는 `unsigned int`로 승격된다.
## 1. 학습 목표
- integer promotion 대상과 결과형을 설명한다.
- 작은 정수 연산의 중간형을 판별한다.
- 저장형과 식의 계산형을 구별한다.
## 2. 선수 지식
기본 정수형과 `sizeof`를 사용한다.
## 3. 핵심 개념
원래 형의 모든 값을 `int`가 표현하면 `int`, 아니면 `unsigned int`로 승격된다. 승격은 값을 보존한다. `unsigned char + unsigned char`가 반드시 작은 unsigned 형에서 계산되는 것은 아니다.
## 4. 문법
```c
int promoted_result = small_a + small_b;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned char a = 200U;
    unsigned char b = 50U;
    int sum = a + b;
    printf("%d\n", sum);
    return 0;
}
```
## 6. 코드 해석
일반적인 구현에서 두 피연산자는 `int`로 승격되어 결과는 250이다. C17의 실제 선택은 `int`의 표현 범위에 따른다.
## 7. 내부 동작
승격은 C 식의 형 규칙이다. 특정 CPU가 작은 레지스터를 쓰는지와 같은 말이 아니다.
## 8. 자주 하는 실수
- 변수의 선언형이 중간 계산형이라고 단정한다.
- 승격을 값 손실 변환으로 생각한다.
- 항상 `int`로 승격된다고 일반화한다.
## 9. 필수 실습
작은 unsigned 두 값의 합을 `int`에 저장한다. [실습 README](../../exercises/06-type-conversions/6-4/README.md).
## 10. 추가 실습
- ★ 기초: `signed char` 덧셈.
- ★★ 응용: `short` 식.
- ★★★ 도전: `int`가 모든 값을 표현하지 못하는 구현 모델.
## 11. 확인 문제
1. promotion 대상은 무엇인가?
2. 언제 `unsigned int`로 승격되는가?
3. 승격은 값을 바꾸는가?
4. 저장형과 계산형은 항상 같은가?
## 12. 핵심 정리
작은 정수형은 식에서 `int` 또는 `unsigned int`로 값 보존 승격된다.
## 13. 다음 Step
[Step 6-5. usual arithmetic conversions](6-5-usual-arithmetic-conversions.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.1.
- [cppreference: integer promotions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC integer implementation](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
