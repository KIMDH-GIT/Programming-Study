# 7-9. 우선순위·결합법칙·평가 순서

우선순위와 결합법칙은 식의 묶임을 정하지만 피연산자 평가 순서를 일반적으로 정하지 않는다.
## 1. 학습 목표
- precedence와 associativity를 설명한다.
- 둘을 evaluation order와 구별한다.
- 괄호로 의도를 명확히 한다.
## 2. 선수 지식
산술·대입·논리 연산자를 사용한다.
## 3. 핵심 개념
`a + b * c`는 `a + (b * c)`로 묶인다. `a = b = 3`은 오른쪽 결합이다. 그러나 함수 인자나 많은 이항 피연산자의 평가 순서는 별도 규칙이며 우선순위 표가 정하지 않는다.
## 4. 문법
```c
int result = a + (b * c);
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int a = 2;
    int b = 3;
    int c = 4;
    printf("%d %d\n", a + b * c, (a + b) * c);
    return 0;
}
```
## 6. 코드 해석
첫 식은 14, 괄호를 바꾼 둘째 식은 20이다. side effect가 없어 평가 순서 문제를 만들지 않는다.
## 7. 내부 동작
파서는 문법 규칙으로 AST 묶임을 정한다. 실행 순서는 C의 별도 sequencing 규칙과 최적화 제약을 따른다.
## 8. 자주 하는 실수
- 높은 우선순위가 먼저 실행됨을 보장한다고 생각한다.
- 결합법칙을 CPU 순서로 해석한다.
- 복잡한 식에 괄호를 아낀다.
## 9. 필수 실습
괄호 두 위치의 결과를 비교한다. [실습 README](../../exercises/07-operators/7-9/README.md).
## 10. 추가 실습
- ★ 기초: 곱셈·덧셈.
- ★★ 응용: 대입 결합.
- ★★★ 도전: 묶임과 sequencing 표.
## 11. 확인 문제
1. precedence는 무엇을 정하는가?
2. associativity는 무엇을 정하는가?
3. 둘이 evaluation order를 보장하는가?
4. 괄호의 장점은?
## 12. 핵심 정리
우선순위와 결합법칙은 문법적 묶임이며 평가 순서와 동일하지 않다.
## 13. 다음 Step
[Step 7-10. Part 7 종합 복습](7-10-part-7-review.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.
- [cppreference: operator precedence](https://en.cppreference.com/w/c/language/operator_precedence.html)
- [cppreference: evaluation order](https://en.cppreference.com/w/c/language/eval_order.html)
