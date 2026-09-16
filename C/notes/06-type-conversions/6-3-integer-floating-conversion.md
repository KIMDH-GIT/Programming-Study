# 6-3. 정수와 부동소수점 사이 변환

정수와 부동소수점은 표현 방식과 정밀도가 달라 양방향 변환 모두 경계를 확인해야 한다.
## 1. 학습 목표
- 정수→실수의 정확성 한계를 설명한다.
- 실수→정수의 0 방향 절단을 설명한다.
- 범위 밖 실수→정수 변환 UB를 피한다.
## 2. 선수 지식
Part 2의 부동소수점과 Step 6-1을 사용한다.
## 3. 핵심 개념
정수→실수에서 값이 정확히 표현되면 유지되고 아니면 인접한 표현값이 구현이 정한 방식으로 선택된다. 유한 실수→정수는 소수 부분을 0 방향으로 버린다. 남은 값이 대상 정수형 범위 밖이면 UB다.
## 4. 문법
```c
double d = integer;
int i = real_value;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int source = 42;
    double exact = source;
    int positive = 3.9;
    int negative = -3.9;
    printf("%.1f %d %d\n", exact, positive, negative);
    return 0;
}
```
## 6. 코드 해석
42는 42.0, 3.9는 3, -3.9는 -3이 된다. 절단은 내림이 아니다.
## 7. 내부 동작
부동소수점 표현과 반올림 방향의 구체적 지원은 구현 특성이지만 C17의 범위·절단 규칙을 우선한다.
## 8. 자주 하는 실수
- 음수도 항상 더 작은 정수로 내린다고 생각한다.
- 모든 큰 정수가 `double`에 정확히 들어간다고 생각한다.
- 범위 밖 실수→정수 결과를 포화값으로 기대한다.
## 9. 필수 실습
양수·음수 실수의 정수 변환을 비교한다. [실습 README](../../exercises/06-type-conversions/6-3/README.md).
## 10. 추가 실습
- ★ 기초: 8.75 변환.
- ★★ 응용: -8.75 변환.
- ★★★ 도전: `DBL_MANT_DIG`와 정수 정밀도 연결.
## 11. 확인 문제
1. -3.9를 `int`로 바꾸면?
2. 절단 방향은?
3. 모든 정수를 실수로 정확히 표현하는가?
4. 실수→정수 범위 밖은 어떻게 분류하는가?
## 12. 핵심 정리
실수→정수는 0 방향 절단이며 범위 밖은 UB다. 정수→실수는 정밀도를 잃을 수 있다.
## 13. 다음 Step
[Step 6-4. integer promotion](6-4-integer-promotion.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.4~5.
- [cppreference: real floating conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [cppreference: numeric limits](https://en.cppreference.com/w/c/types/limits.html)
