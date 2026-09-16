# 6-1. implicit conversion

C는 대입·초기화·연산·함수 호출에서 문맥이 요구하는 형으로 값을 자동 변환한다.
## 1. 학습 목표
- implicit conversion이 일어나는 문맥을 찾는다.
- 값 변환과 객체 표현 재해석을 구별한다.
- 변환 뒤 정보 손실 가능성을 설명한다.
## 2. 선수 지식
Part 2의 자료형과 Part 5의 계산식을 사용한다.
## 3. 핵심 개념
`double d = 7;`에서는 `int` 값 7이 `double` 값 7.0으로 변환된다. `int i = 3.9;`에서는 소수 부분이 0 방향으로 제거된다. 변환은 수학적 값을 대상형 값으로 바꾸며 메모리 bit를 그대로 다시 읽는 행위가 아니다.
## 4. 문법
```c
double wider = 7;
int narrower = 3.9;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int count = 7;
    double measured = count;
    int whole = 3.9;
    printf("%d %.1f %d\n", count, measured, whole);
    return 0;
}
```
## 6. 코드 해석
`count`는 7.0으로 정확히 변환되고 3.9는 3으로 변환된다. 초기화에서 대상 객체형이 변환 목적형을 정한다.
## 7. 내부 동작
**[C17 표준]** 변환 규칙이 결과값을 정한다. 컴파일러는 결과가 같다면 별도 명령 없이 처리할 수 있다.
## 8. 자주 하는 실수
- 자동 변환은 항상 손실이 없다고 생각한다.
- 초기화를 대입문이라고 부른다.
- 값 변환을 bit 재해석으로 설명한다.
## 9. 필수 실습
정수→실수와 실수→정수 초기화를 관찰한다. [실습 README](../../exercises/06-type-conversions/6-1/README.md)를 따른다.
## 10. 추가 실습
- ★ 기초: 12를 `double`로 초기화한다.
- ★★ 응용: -3.9를 `int`로 변환한다.
- ★★★ 도전: 손실 가능 문맥을 표로 만든다.
## 11. 확인 문제
1. implicit conversion은 무엇인가?
2. `int x = 3.9;`의 값은 무엇인가?
3. 초기화와 대입은 같은 문법인가?
4. 변환은 객체 표현 재해석인가?
## 12. 핵심 정리
암시적 변환은 문맥이 요구하는 대상형의 값으로 바꾸며 손실 가능성을 따져야 한다.
## 13. 다음 Step
[Step 6-2. 정수형 범위 축소와 범위 밖 변환](6-2-integer-narrowing.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.
- [cppreference: conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC conversion warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
