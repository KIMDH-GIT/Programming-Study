# 6-10. Part 6 종합 복습
Part 6은 값이 언제 어떤 형으로 바뀌며 중간 결과에 어떤 영향을 주는지 연결했다.
## 1. 학습 목표
- implicit·explicit conversion을 구별한다.
- promotions와 usual conversions를 순서대로 설명한다.
- 범위·정밀도·UB 경계를 판별한다.
## 2. 선수 지식
Step 6-1~6-9 전체다.
## 3. 핵심 개념
작은 정수는 promotion되고, 산술 피연산자는 공통형으로 변환된다. 정수 축소·실수 변환은 범위와 정밀도를 확인한다. cast는 의도를 명시하지만 안전 검사를 대신하지 않는다.
## 4. 문법
```c
double average = (double)total / count;
```
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    unsigned char small = (unsigned char)((unsigned int)UCHAR_MAX + 1U);
    int total = 5;
    int count = 2;
    printf("%u %.1f %d\n", (unsigned int)small,
           (double)total / count, -5 / 2);
    return 0;
}
```
## 6. 코드 해석
unsigned 축소는 0, 이른 cast 평균은 2.5, 음수 정수 몫은 -2다.
## 7. 내부 동작
변환은 C 추상 기계의 값 규칙이다. CPU 명령과 일대일 대응하지 않는다.
## 8. 자주 하는 실수
- 모든 변환이 안전하다고 생각한다.
- signed 범위 밖 결과를 wrap으로 단정한다.
- 늦은 cast로 손실을 복구하려 한다.
## 9. 필수 실습
세 핵심 변환을 한 프로그램에서 관찰한다. [실습 README](../../exercises/06-type-conversions/6-10/README.md).
## 10. 추가 실습
- ★ 기초: 변환 방향표.
- ★★ 응용: 공통형 추적.
- ★★★ 도전: UB 없는 경계표.
## 11. 확인 문제
1. integer promotion은 언제 일어나는가?
2. 실수→정수는 어느 방향으로 절단하는가?
3. cast가 범위 검사를 하는가?
4. signed·unsigned 공통형은 어떻게 정하는가?
5. `(double)(5/2)`와 `(double)5/2`의 차이는?
## 12. 핵심 정리
각 식의 변환 순서·공통형·범위를 추적해야 결과와 위험을 정확히 설명할 수 있다.
## 13. 다음 Step
Step 7-1. 식·피연산자·결과값
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1, 6.5.4~5.
- [cppreference: conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC warning options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
