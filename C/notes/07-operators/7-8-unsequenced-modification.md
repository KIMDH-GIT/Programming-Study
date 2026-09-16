# 7-8. 한 식에서 같은 객체를 여러 번 변경하는 위험

같은 객체에 대한 side effect가 서로 순서화되지 않으면 C17에서 Undefined Behavior가 될 수 있다.
## 1. 학습 목표
- value computation과 side effect를 구별한다.
- unsequenced modification UB를 판별한다.
- 복잡한 식을 여러 statement로 나눈다.
## 2. 선수 지식
증감·대입 연산자를 사용한다.
## 3. 핵심 개념
두 sequence point 사이에서 같은 scalar 객체를 두 번 변경하거나, 변경과 그 값을 다른 목적으로 읽는 행위가 순서화되지 않으면 UB다. `i = i++ + ++i;`의 결과를 예측하지 않는다.
## 4. 문법
```c
/* 안전한 분리 */
i++;
int snapshot = i;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int value = 5;
    value++;
    int snapshot = value;
    value += 2;
    printf("%d %d\n", snapshot, value);
    return 0;
}
```
## 6. 코드 해석
각 변경은 별도 full expression에 있어 순서가 명확하다. 결과는 6과 8이다.
## 7. 내부 동작
UB에서는 컴파일러가 특정 실행 순서를 보장할 의무가 없다. CPU 결과를 관찰해 규칙을 만들지 않는다.
## 8. 자주 하는 실수
- 왼쪽부터 평가된다고 가정한다.
- warning이 없으면 정의된 식이라고 생각한다.
- UB 식을 여러 번 실행해 답을 찾는다.
## 9. 필수 실습
위험한 식을 실행하지 않고 안전한 statement로 분해한다. [실습 README](../../exercises/07-operators/7-8/README.md).
## 10. 추가 실습
- ★ 기초: 변경 하나.
- ★★ 응용: 복잡한 식 분해.
- ★★★ 도전: sequence 규칙 조사.
## 11. 확인 문제
1. side effect란?
2. 위험한 식의 결과를 예측할 수 있는가?
3. warning 부재가 안전을 보장하는가?
4. 가장 단순한 해결법은?
## 12. 핵심 정리
한 full expression에서 같은 객체를 여러 번 변경하지 말고 statement로 분리한다.
## 13. 다음 Step
[Step 7-9. 우선순위·결합법칙·평가 순서](7-9-precedence-associativity-order.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 5.1.2.3, 6.5.
- [cppreference: evaluation order](https://en.cppreference.com/w/c/language/eval_order.html)
- [GCC warning options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
