# 7-7. 전위·후위 `++`, `--`

증감 연산자는 객체 값을 1만큼 바꾸며 전위와 후위는 식의 결과값이 다르다.
## 1. 학습 목표
- 전위·후위 결과값을 구별한다.
- 객체 변경 side effect를 설명한다.
- 한 full expression에서 단순하게 사용한다.
## 2. 선수 지식
대입과 expression statement를 사용한다.
## 3. 핵심 개념
`++x`는 값을 바꾼 뒤 새 값을 결과로, `x++`는 바꾸기 전 값을 결과로 만든다. `--`도 같다. 둘 다 객체를 변경한다.
## 4. 문법
```c
int prefix = ++value;
int postfix = value++;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int value = 5;
    int before = value++;
    int after = ++value;
    printf("%d %d %d\n", before, after, value);
    return 0;
}
```
## 6. 코드 해석
후위 결과는 5이고 value는 6, 전위 뒤 결과와 value는 7이다.
## 7. 내부 동작
값 계산과 side effect를 구별한다. 복잡한 한 식의 평가 순서는 Step 7-8~9에서 다룬다.
## 8. 자주 하는 실수
- 전위와 후위가 항상 같은 결과값이라고 생각한다.
- 변경 가능한 객체가 아닌 값에 적용한다.
- 한 식에서 여러 번 변경한다.
## 9. 필수 실습
전위·후위를 별도 statement에서 비교한다. [실습 README](../../exercises/07-operators/7-7/README.md).
## 10. 추가 실습
- ★ 기초: 전위 증가.
- ★★ 응용: 후위 감소.
- ★★★ 도전: 값 계산·side effect 표.
## 11. 확인 문제
1. `x++` 결과값은 언제 값인가?
2. `++x` 결과값은?
3. 두 연산 모두 객체를 변경하는가?
4. 복잡한 한 식을 피하는 이유는?
## 12. 핵심 정리
전위는 변경 뒤 값, 후위는 변경 전 값을 결과로 하며 둘 다 객체를 변경한다.
## 13. 다음 Step
[Step 7-8. 한 식에서 같은 객체를 여러 번 변경하는 위험](7-8-unsequenced-modification.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.4, 6.5.3.1.
- [cppreference: increment/decrement](https://en.cppreference.com/w/c/language/operator_incdec.html)
- [cppreference: evaluation order](https://en.cppreference.com/w/c/language/eval_order.html)
