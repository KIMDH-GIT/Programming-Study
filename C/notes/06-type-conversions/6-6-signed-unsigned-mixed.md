# 6-6. signed·unsigned 혼합 연산
같은 rank의 signed·unsigned를 섞으면 signed 값이 unsigned로 변환될 수 있다.
## 1. 학습 목표
- 혼합 공통형을 판별한다.
- 음수의 unsigned 변환을 설명한다.
- 예상 밖 큰 결과를 피한다.
## 2. 선수 지식
Step 6-2, 6-5를 사용한다.
## 3. 핵심 개념
같은 rank라면 unsigned형이 공통형이 된다. `-1`을 `unsigned int`로 바꾸면 `UINT_MAX`가 된다. 비교 연산은 Part 7이므로 여기서는 변환된 값만 관찰한다.
## 4. 문법
```c
unsigned int converted = -1;
```
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    int negative = -1;
    unsigned int converted = negative;
    printf("%u %u\n", converted, UINT_MAX);
    return 0;
}
```
## 6. 코드 해석
두 출력은 같다. 대입 전 값 변환이 일어난다.
## 7. 내부 동작
모듈러 값 규칙이지 bit 자르기 설명이 표준의 본질은 아니다.
## 8. 자주 하는 실수
- unsigned가 음수를 그대로 저장한다고 생각한다.
- 모든 혼합에서 unsigned가 이긴다고 과도하게 일반화한다.
- 논리 오류와 UB를 혼동한다.
## 9. 필수 실습
`-1`의 unsigned 변환을 관찰한다. [실습 README](../../exercises/06-type-conversions/6-6/README.md).
## 10. 추가 실습
- ★ 기초: 0 변환.
- ★★ 응용: -2 변환.
- ★★★ 도전: rank가 다른 혼합표.
## 11. 확인 문제
1. `-1`을 `unsigned int`로 바꾸면?
2. 왜 `UINT_MAX`인가?
3. 모든 혼합이 무조건 unsigned인가?
4. 이 변환 자체가 UB인가?
## 12. 핵심 정리
signed·unsigned 혼합은 rank와 표현 범위 규칙으로 공통형을 정한다.
## 13. 다음 Step
[Step 6-7. explicit cast 문법](6-7-explicit-cast.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.3, 6.3.1.8.
- [cppreference: conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC sign-conversion warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
