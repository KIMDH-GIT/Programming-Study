# Step 3-6. 2의 보수 모델과 C17이 허용하는 signed 표현

2의 보수 원리를 이해하되 이를 C17의 유일한 signed 표현으로 일반화하지 않는다.

## 1. 학습 목표
- 2의 보수 자릿값으로 양수·음수를 해석한다.
- 범위 비대칭과 덧셈 회로의 장점을 설명한다.
- C17의 세 signed 표현과 예외 패턴을 구별한다.
- 실제 범위는 `<limits.h>`로 확인한다.

## 2. 선수 지식
[3-5](3-5-n-bit-unsigned-representation.md)의 N-bit unsigned 모형과 Part 2의 signed 범위를 사용한다.

## 3. 핵심 개념
**[모든 패턴이 정상값인 8-bit two's-complement 구현 예]**
```text
00000001 = 1
11111111 = -1
11111110 = -2
10000000 = -128
01111111 = 127
```
최상위 자릿값을 `-2^7`로, 나머지를 양의 자릿값으로 읽는다. 따라서 일반적인 N-bit 2의 보수 모형 범위는 `-2^(N-1) ~ 2^(N-1)-1`이다. 단순한 반전 절차를 외우기보다, unsigned 원 위에서 `2^N-x`가 `-x`와 같은 덧셈 결과를 만든다는 관계가 핵심이다. 같은 가산 회로의 하위 N bit를 활용할 수 있어 널리 사용된다.

**[C17 표준]** signed 정수 표현은 부호와 크기, 1의 보수, 2의 보수 중 하나일 수 있다. 부호와 크기의 sign-only 패턴과 1의 보수 all-ones 패턴은 음의 0 또는 trap일 수 있다. C17에서는 2의 보수 sign-only 패턴도 추가 음수 정상값 또는 trap일 수 있다. padding도 가능하지만 `signed char`에는 padding이 없다. 따라서 C17만으로 모든 signed 범위를 2의 보수 공식으로 확정하지 않는다.

## 4. 문법
표현을 강제로 고르는 C 문법은 없다. `INT_MIN`, `INT_MAX`로 실제 범위를 읽고 `%d`로 출력한다.

## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    int positive = 1;
    int negative = -1;
    printf("values=%d %d\n", positive, negative);
    printf("int range=%d..%d\n", INT_MIN, INT_MAX);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_model.c -o signed_model && ./signed_model
```

## 6. 코드 해석
값 1과 -1은 안전하게 표현된다. 한계 매크로는 구현 범위를 보여 주지만 그 숫자만으로 padding이나 모든 객체 패턴을 관찰한 것은 아니다.

## 7. 내부 동작
현대 x86-64, ARM, RISC-V 구현은 일반적으로 2의 보수를 사용한다. 이는 구현 관찰이다. C 추상 기계는 허용된 표현과 값 규칙을 제공하며 CPU 회로나 레지스터 폭을 고정하지 않는다.

## 8. 자주 하는 실수
- C17 signed가 항상 2의 보수라고 한다.
- 2의 보수를 반전 후 +1로만 암기한다.
- 최상위 bit를 모든 상황에서 단순 부호 표지라고 한다.
- `INT_MIN`을 항상 `-INT_MAX-1`로 만든다.
- trap·negative zero 가능성을 C23 규칙과 섞는다.

## 9. 필수 실습
[README](../../exercises/03-integer-representation/3-6/README.md)에 따라 세 표현 모형의 패턴표를 종이에 작성하고 실제 한계를 안전하게 출력한다.

## 10. 추가 실습
- ★ 8-bit 2의 보수에서 -5 계산
- ★★ 세 모형에서 `10000101` 비교
- ★★★ 구현 문서에서 signed 표현 확인

## 11. 확인 문제
1. 모든 패턴이 정상값인 일반적인 8-bit 2의 보수 범위는 무엇인가?
2. 그 범위는 왜 비대칭인가?
3. C17이 허용하는 세 signed 표현은 무엇인가?
4. C17에서 sign-only 2의 보수 패턴은 반드시 -128인가?
5. 실제 `int` 범위는 어디서 읽는가?
<details><summary>정답</summary>
1. -128~127. 2. 0이 하나이고 최상위 음의 자릿값이 있기 때문이다. 3. 부호와 크기, 1의 보수, 2의 보수. 4. 아니다. trap일 수 있다. 5. `<limits.h>`.
</details>

## 12. 핵심 정리
2의 보수는 modulo 관계와 가산 회로 재사용이 장점인 널리 쓰이는 구현 모델이다. C17은 이를 유일한 표현으로 강제하지 않는다.

## 13. 다음 Step
[Step 3-7. 같은 비트 패턴의 signed·unsigned 해석](3-7-same-bits-signed-unsigned.md)

## 14. 참고 자료
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 6.2.6.2. C11 공개 초안이며 C17 공통 규칙 참고 자료다.
- [GCC Integers](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
