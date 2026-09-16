# 7-5. `&&`, `||`, `!`와 short-circuit

논리 연산은 0을 거짓, 0이 아닌 값을 참으로 취급하고 결과로 `int` 0 또는 1을 만든다.
## 1. 학습 목표
- `!`, `&&`, `||`의 진릿값을 설명한다.
- short-circuit 평가를 설명한다.
- 오른쪽 평가를 안전 조건에 연결한다.
## 2. 선수 지식
비교 결과와 나눗셈 경계를 사용한다.
## 3. 핵심 개념
`!x`는 x가 0이면 1, 아니면 0이다. `&&`는 왼쪽이 0이면 오른쪽을 평가하지 않는다. `||`는 왼쪽이 0이 아니면 오른쪽을 평가하지 않는다. 결과형은 `int`다.
## 4. 문법
```c
int safe = divisor != 0 && dividend / divisor > 2;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int dividend = 12;
    int divisor = 0;
    int safe = divisor != 0 && dividend / divisor > 2;
    printf("%d %d %d\n", safe, !divisor, divisor == 0 || dividend > 0);
    return 0;
}
```
## 6. 코드 해석
왼쪽 비교가 거짓이므로 위험한 나눗셈은 평가되지 않는다. 결과는 0, 1, 1이다.
## 7. 내부 동작
short-circuit는 C가 보장하는 평가 규칙이다. 컴파일러는 관찰 가능한 규칙을 보존해야 한다.
## 8. 자주 하는 실수
- 0이 아닌 참 결과가 그대로 반환된다고 생각한다.
- `&`, `|`와 혼동한다.
- 오른쪽이 항상 평가된다고 생각한다.
## 9. 필수 실습
0 제수를 short-circuit로 평가하지 않는 식을 출력한다. [실습 README](../../exercises/07-operators/7-5/README.md).
## 10. 추가 실습
- ★ 기초: `!0`, `!5`.
- ★★ 응용: `&&`, `||` 진리표.
- ★★★ 도전: 안전 guard 식 분석.
## 11. 확인 문제
1. C에서 거짓 값은?
2. 논리 결과형은?
3. `&&`가 오른쪽을 생략하는 조건은?
4. `||`가 오른쪽을 생략하는 조건은?
## 12. 핵심 정리
논리 연산 결과는 `int` 0/1이며 `&&`, `||`는 왼쪽 값에 따라 오른쪽 평가를 생략한다.
## 13. 다음 Step
[Step 7-6. `=`, `+=`, `-=`, `*=`, `/=`, `%=`](7-6-assignment-operators.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.3, 6.5.13~14.
- [cppreference: logical operators](https://en.cppreference.com/w/c/language/operator_logical.html)
- [cppreference: evaluation order](https://en.cppreference.com/w/c/language/eval_order.html)
