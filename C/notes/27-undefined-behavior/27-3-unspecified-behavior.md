# 27-3. unspecified behavior
## 1. 학습 목표
- unspecified behavior를 둘 이상의 허용된 가능성으로 설명한다.
- unspecified evaluation order와 unsequenced side effects를 구분한다.
- 결과가 여러 개여도 프로그램이 defined일 수 있음을 이해한다.
## 2. 선수 지식
Part 7의 우선순위·평가 순서와 함수 호출을 안다.
## 3. 핵심 개념
**[C17 — Unspecified]** 표준이 둘 이상의 가능성을 허용하고 어느 것을 택할지 더 요구하지 않는 behavior다. 구현은 선택을 문서화할 의무가 없다.

함수 인자의 평가 순서는 unspecified다. 그러나 인자들이 서로의 객체를 변경하지 않으면 어느 순서라도 defined다. 반대로 `f(i++, i++)`는 단순히 “순서가 unspecified”인 데서 끝나지 않고, 같은 객체에 대한 side effects가 서로 unsequenced이므로 UB다.
## 4. 문법
```c
show(left(), right());
```
`left()`와 `right()` 중 무엇을 먼저 평가하는지는 정해지지 않는다.

⚠ 분석용 — 실행하지 않는다.
```c
use(i++, i++); /* 같은 객체의 unsequenced modifications: UB */
```
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int left(void)
{
    return 10;
}

static int right(void)
{
    return 20;
}

static void show(int a, int b)
{
    printf("%d %d\n", a, b);
}

int main(void)
{
    show(left(), right());
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o unspecified
./unspecified
```
## 6. 코드 해석
인자 평가 순서와 무관하게 `show`가 받는 값은 각각 10과 20이므로 출력은 `10 20`이다. unspecified order 자체는 UB가 아니다.
## 7. 내부 동작
**[C17]** 함수 designator와 각 actual argument의 평가는 actual call 전에 모두 끝난다. designator와 argument들 사이의 평가 순서는 unspecified이고 이 평가들은 서로 unsequenced다. 같은 scalar object에 대한 unsequenced side effect 충돌은 별도 UB 규칙이다.

**[compiler observation]** compiler나 optimization level마다 평가 순서가 달라질 수 있다. 한 번 관찰한 순서를 계약으로 사용하지 않는다.
## 8. 자주 하는 실수
- 결과 후보가 여러 개면 모두 UB라고 한다.
- unspecified order 자체를 unsequenced side-effect UB와 동일시한다.
- 우선순위가 evaluation order를 정한다고 생각한다.
- 한 compiler의 평가 순서를 언어 보장으로 기록한다.
## 9. 필수 실습
독립적인 두 함수를 인자로 전달하고, 순서와 최종 값의 보장을 별도 문장으로 쓴다.
[27-3 exercise](../../exercises/27-undefined-behavior/27-3/README.md)
## 10. 추가 실습
- ★ 두 함수에 서로 다른 메시지를 추가해 현재 순서를 관찰한다.
- ★★ side effect 없는 식으로 인자 순서 의존성을 제거한다.
- ★★★ unspecified·implementation-defined·UB 비교표를 완성한다.
## 11. 확인 문제
1. unspecified behavior의 가능성은 누가 정하는가?
2. 구현은 선택을 문서화해야 하는가?
3. `show(left(), right())`가 UB가 아닌 이유는?
4. `use(i++, i++)`의 핵심 문제는 무엇인가?
5. 우선순위와 평가 순서는 왜 다른가?
## 12. 핵심 정리
- unspecified behavior는 허용된 복수 가능성 중 선택이다.
- argument order가 unspecified라는 사실만으로 UB가 되지는 않는다.
- unsequenced side effects는 별도 규칙으로 분석한다.
## 13. 다음 Step
[27-4. signed integer overflow](27-4-signed-integer-overflow.md)
## 14. 참고 자료
- N1570 3.4.4, 6.5p2, 6.5.2.2p10. N1570은 **C11 공개 Committee Draft**이며 sequencing 규칙은 C17에서도 유지된다.
- [cppreference: Order of evaluation](https://en.cppreference.com/w/c/language/eval_order)
