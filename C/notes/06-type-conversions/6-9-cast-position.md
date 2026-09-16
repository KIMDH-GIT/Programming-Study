# 6-9. cast 위치와 중간 결과
cast는 적용된 하위 식의 결과만 바꾼다. 손실이 일어난 뒤 cast하면 잃은 부분을 되살릴 수 없다.
## 1. 학습 목표
- cast 적용 범위를 괄호로 읽는다.
- 중간 결과형을 추적한다.
- 나눗셈 전후 cast를 구별한다.
## 2. 선수 지식
Step 6-7~6-8을 사용한다.
## 3. 핵심 개념
`(double)(5/2)`는 정수 나눗셈 2를 만든 뒤 2.0으로 바꾼다. `(double)5/2`는 먼저 5.0을 만들어 실수 나눗셈 2.5를 한다.
## 4. 문법
```c
double late = (double)(a / b);
double early = (double)a / b;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int a = 5;
    int b = 2;
    printf("%.1f %.1f\n", (double)(a / b), (double)a / b);
    return 0;
}
```
## 6. 코드 해석
첫 결과는 2.0, 둘째는 2.5다. 괄호가 cast 피연산자를 정한다.
## 7. 내부 동작
컴파일러는 최적화할 수 있지만 C가 정한 각 하위 식의 값과 observable output을 보존해야 한다.
## 8. 자주 하는 실수
- 최종 대입형이 중간 손실을 복구한다고 생각한다.
- cast 범위를 보지 않는다.
- 우선순위를 평가 순서 전체와 혼동한다.
## 9. 필수 실습
두 cast 위치의 결과를 비교한다. [실습 README](../../exercises/06-type-conversions/6-9/README.md).
## 10. 추가 실습
- ★ 기초: 7과 2.
- ★★ 응용: 변수 중 하나를 double로 선언.
- ★★★ 도전: 중간형 표.
## 11. 확인 문제
1. `(double)(5/2)`는?
2. `(double)5/2`는?
3. 늦은 cast가 소수 부분을 복구하는가?
4. 괄호는 무엇을 정하는가?
## 12. 핵심 정리
원하는 계산 전에 cast해야 공통형과 중간 결과가 달라진다.
## 13. 다음 Step
[Step 6-10. Part 6 종합 복습](6-10-part-6-review.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.4~5.
- [cppreference: cast](https://en.cppreference.com/w/c/language/cast.html)
- [cppreference: conversions](https://en.cppreference.com/w/c/language/conversion.html)
