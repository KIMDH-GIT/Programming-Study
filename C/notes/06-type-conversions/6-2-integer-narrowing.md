# 6-2. 정수형 범위 축소와 범위 밖 변환

정수값을 더 좁은 범위의 정수형으로 바꿀 때 대상형이 그 값을 표현할 수 있는지 먼저 확인해야 한다.
## 1. 학습 목표
- 표현 가능한 정수 변환과 범위 밖 변환을 구별한다.
- unsigned 대상 변환의 모듈러 규칙을 설명한다.
- signed 대상의 구현 정의 결과를 정확히 분류한다.
## 2. 선수 지식
Part 2의 범위와 Part 3의 unsigned 모듈러를 사용한다.
## 3. 핵심 개념
대상형이 값을 표현하면 값은 유지된다. unsigned 대상은 그 형의 값 개수로 반복해 범위 안 값이 된다. signed 대상이 값을 표현하지 못하면 결과가 구현 정의이거나 구현 정의 신호가 발생할 수 있다. signed overflow 산술과 같은 규칙이 아니다.
## 4. 문법
```c
unsigned char reduced = 300U;
```
`UCHAR_MAX`가 255인 구현이라면 값은 44지만 그 숫자를 모든 구현에 일반화하지 않는다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    unsigned int source = (unsigned int)UCHAR_MAX + 1U;
    unsigned char reduced = source;
    printf("%u %u\n", source, (unsigned int)reduced);
    return 0;
}
```
## 6. 코드 해석
`source`는 `UCHAR_MAX+1`, unsigned char로 변환한 결과는 0이다. 출력용으로 다시 `unsigned int`로 변환한다.
## 7. 내부 동작
표준은 값 관계를 정하지만 저장 명령과 레지스터 폭은 구현이 선택한다.
## 8. 자주 하는 실수
- 모든 축소를 하위 bit 자르기로 설명한다.
- signed 범위 밖 결과도 wrap한다고 단정한다.
- 변환과 signed 산술 overflow를 혼동한다.
## 9. 필수 실습
`UCHAR_MAX+1`의 unsigned 변환을 관찰한다. [실습 README](../../exercises/06-type-conversions/6-2/README.md)를 따른다.
## 10. 추가 실습
- ★ 기초: 범위 안 값을 변환한다.
- ★★ 응용: `UCHAR_MAX+2`를 관찰한다.
- ★★★ 도전: signed 대상 규칙을 정리한다.
## 11. 확인 문제
1. 대상형이 값을 표현하면 결과는 무엇인가?
2. unsigned 대상 범위 밖 변환 규칙은 무엇인가?
3. signed 대상 범위 밖 결과는 항상 wrap하는가?
4. signed overflow와 같은 개념인가?
## 12. 핵심 정리
정수 축소는 대상 범위와 signed 여부에 따라 규칙이 다르다.
## 13. 다음 Step
[Step 6-3. 정수와 부동소수점 사이 변환](6-3-integer-floating-conversion.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.3.
- [cppreference: implicit conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC integer implementation](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
