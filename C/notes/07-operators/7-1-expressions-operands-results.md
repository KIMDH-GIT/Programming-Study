# 7-1. 식·피연산자·결과값

연산자는 피연산자에 적용되어 값을 만드는 식을 구성한다. 식과 statement는 같은 개념이 아니다.
## 1. 학습 목표
- operator, operand, expression, result type을 구별한다.
- 식과 expression statement를 구별한다.
- Part 6 변환이 연산 전에 적용됨을 설명한다.
## 2. 선수 지식
Part 6의 promotion과 usual arithmetic conversions를 사용한다.
## 3. 핵심 개념
`a + b`에서 `+`는 연산자, `a`와 `b`는 피연산자, 전체는 식이다. `a + b;`는 식 뒤에 세미콜론을 붙인 expression statement다. 결과형은 단순히 왼쪽 변수형이 아니라 연산자와 변환 규칙으로 정해진다.
## 4. 문법
```c
int result = left + right;
left + right;
```
첫 줄은 초기화를 포함한 declaration, 둘째 줄은 expression statement다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int left = 7;
    double right = 0.5;
    double result = left + right;
    printf("%.1f\n", result);
    return 0;
}
```
## 6. 코드 해석
`left`가 `double`로 변환된 뒤 덧셈 결과 7.5가 만들어진다.
## 7. 내부 동작
C 식은 추상 기계 규칙이다. 한 식이 CPU 명령 하나와 일대일 대응한다고 단정하지 않는다.
## 8. 자주 하는 실수
- 식과 statement를 같은 말로 쓴다.
- 연산자와 피연산자를 혼동한다.
- 결과형을 대입 대상형이 먼저 정한다고 생각한다.
## 9. 필수 실습
두 형의 피연산자와 결과형을 표로 기록한다. [실습 README](../../exercises/07-operators/7-1/README.md).
## 10. 추가 실습
- ★ 기초: 정수식 구성.
- ★★ 응용: `int + double`.
- ★★★ 도전: 하위 식 형 추적.
## 11. 확인 문제
1. `a + b`의 operator는?
2. operand는 무엇인가?
3. 식과 expression statement의 차이는?
4. 결과형은 무엇으로 정하는가?
## 12. 핵심 정리
연산자와 피연산자가 식을 만들고, 식은 값과 형을 가진다.
## 13. 다음 Step
[Step 7-2. `+`, `-`, `*`, `/`, `%`](7-2-arithmetic-operators.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.
- [cppreference: operators](https://en.cppreference.com/w/c/language/operator_precedence.html)
- [cppreference: expressions](https://en.cppreference.com/w/c/language/expressions.html)
