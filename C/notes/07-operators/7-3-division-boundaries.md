# 7-3. 나눗셈 경계: 0과 `INT_MIN / -1`

정수 `/`와 `%`에는 실행 결과를 예측하면 안 되는 두 핵심 UB 경계가 있다.
## 1. 학습 목표
- 0 제수 UB를 설명한다.
- `INT_MIN / -1` 경계를 설명한다.
- UB 예제를 실행하지 않고 분석한다.
## 2. 선수 지식
`INT_MIN`, signed 범위, 정수 나눗셈을 사용한다.
## 3. 핵심 개념
두 번째 피연산자가 0인 `/`, `%`는 UB다. 또한 몫 `a/b`가 결과형으로 표현 불가능하면 `/`와 대응 `%` 모두 UB다. `INT_MIN / -1`의 수학적 결과는 `INT_MAX+1`이어서 `int`로 표현되지 않는다.
## 4. 문법
```c
/* 실행 금지: value / 0, INT_MIN / -1 */
```
안전한 예는 0이 아닌 제수와 표현 가능한 몫을 사용한다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    int safe_dividend = INT_MIN + 1;
    int divisor = -1;
    printf("%d\n", safe_dividend / divisor);
    return 0;
}
```
## 6. 코드 해석
`INT_MIN+1`의 반대값은 `INT_MAX`이므로 표현 가능하다. 위험 경계는 실행하지 않는다.
## 7. 내부 동작
CPU trap 여부와 무관하게 C 의미는 UB다. 컴파일러는 UB가 없다고 가정해 최적화할 수 있다.
## 8. 자주 하는 실수
- 0 나눗셈을 단순 runtime error라고만 부른다.
- `INT_MIN/-1`이 wrap한다고 단정한다.
- 컴파일러 진단이 없으면 안전하다고 생각한다.
## 9. 필수 실습
안전 경계와 UB 경계를 표로 분석한다. [실습 README](../../exercises/07-operators/7-3/README.md).
## 10. 추가 실습
- ★ 기초: 안전한 제수 목록.
- ★★ 응용: `INT_MIN+1`.
- ★★★ 도전: UB 최적화 이유 조사.
## 11. 확인 문제
1. `a/0`의 분류는?
2. `a%0`의 분류는?
3. `INT_MIN/-1`이 위험한 이유는?
4. UB 결과를 특정할 수 있는가?
## 12. 핵심 정리
0 제수와 표현 불가능한 몫은 UB이며 교육 실습에서 실행하지 않는다.
## 13. 다음 Step
[Step 7-4. `==`, `!=`, `<`, `>`, `<=`, `>=`](7-4-comparison-operators.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [GCC optimizer and UB](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
