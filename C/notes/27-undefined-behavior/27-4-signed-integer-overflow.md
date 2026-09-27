# 27-4. signed integer overflow
## 1. 학습 목표
- signed integer 결과가 표현 범위를 벗어날 때의 UB를 설명한다.
- unsigned modulo arithmetic과 구분한다.
- 덧셈·나눗셈·shift 경계를 실행 전에 검사한다.
## 2. 선수 지식
Part 3의 정수 표현, Part 7의 산술 경계, Part 21의 shift를 안다.
## 3. 핵심 개념
**[C17 — Undefined Behavior]** signed arithmetic 결과가 결과 type으로 표현되지 않으면 UB다.

⚠ 분석용 — 실행하지 않는다.
```c
int x = INT_MAX;
x = x + 1; /* signed overflow: UB */
```

2의 보수 machine에서 wrap처럼 보일 수 있어도 C17 보장이 아니다. 반면 unsigned arithmetic은 해당 type 범위의 modulo로 규정된다.
## 4. 문법
안전한 덧셈은 연산 전에 검사한다.
```c
if (b > 0 && a > INT_MAX - b) {
    /* overflow 처리 */
} else {
    int sum = a + b;
}
```

⚠ 분석용 — 실행하지 않는다.
```c
int q1 = 10 / 0;         /* divisor zero: UB */
int q2 = INT_MIN / -1;   /* quotient가 int에 표현되지 않으면 UB */
int s1 = 1 << -1;        /* negative shift count: UB */
int s2 = 1 << 31;        /* 32-bit int 가정 시 표현 불가: UB */
```
shift count가 음수이거나 promoted left operand 폭 이상인 경우도 UB다. 음수 signed value의 right shift는 implementation-defined이지 UB가 아니다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>

static int checked_add(int a, int b, int *result)
{
    if ((b > 0 && a > INT_MAX - b) ||
        (b < 0 && a < INT_MIN - b)) {
        return 0;
    }
    *result = a + b;
    return 1;
}

int main(void)
{
    int sum;

    if (checked_add(20, 22, &sum)) {
        printf("%d\n", sum);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o checked_add
./checked_add
```
## 6. 코드 해석
검사를 먼저 수행하므로 `a + b`는 표현 가능한 경우에만 평가된다. 출력은 `42`다.
## 7. 내부 동작
**[C17]** signed overflow, 0으로 integer division, 표현 불가능한 quotient, 잘못된 shift는 각 연산 규칙의 UB다. unsigned 연산은 modulo semantics를 가진다.

**[optimizer]** defined execution에서는 signed overflow가 없다는 전제로 `x + 1 > x` 같은 식을 단순화할 수 있다.

**[CPU / ISA]** machine instruction이 wrap하거나 trap해도 signed C source의 UB를 defined로 바꾸지 않는다.

**[MIPS — 수업 기준]** `add`/`addu`의 trap 차이는 ISA 의미이며 compiler가 C `+`를 반드시 한 instruction으로 번역한다는 뜻이 아니다.

**[RISC-V — 병행 학습]** integer instruction의 modulo 결과와 C signed-overflow 의미를 분리한다.
## 8. 자주 하는 실수
- signed overflow는 항상 최솟값으로 wrap한다고 한다.
- unsigned modulo arithmetic을 signed overflow와 같은 UB로 분류한다.
- `INT_MIN / -1`을 two's complement wrap으로 설명한다.
- `1 << 31`을 32-bit signed `int`에서 정상이라고 한다.
- negative signed right shift를 UB라고 한다.
## 9. 필수 실습
`checked_add`를 작성하고 성공·실패 입력을 검사하되, overflow 식 자체는 평가하지 않는다.
[27-4 exercise](../../exercises/27-undefined-behavior/27-4/README.md)
## 10. 추가 실습
- ★ checked subtraction을 작성한다.
- ★★ divisor 0과 `INT_MIN / -1`을 연산 전에 거부한다.
- ★★★ shift count와 left operand를 함께 검증한다.
## 11. 확인 문제
1. signed overflow의 C17 분류는?
2. unsigned arithmetic은 왜 같은 의미의 overflow가 아닌가?
3. `INT_MIN / -1`이 문제가 되는 조건은?
4. 유효한 shift count 범위는?
5. hardware wrap이 C 결과를 보장하지 않는 이유는?
## 12. 핵심 정리
- signed 결과가 type에 표현되지 않으면 UB다.
- 연산 후가 아니라 연산 전에 범위를 검사한다.
- unsigned, signed right shift, CPU 동작을 각각 별도 규칙으로 본다.
## 13. 다음 Step
[27-5. array out-of-bounds](27-5-array-out-of-bounds.md)
## 14. 참고 자료
- N1570 6.2.5p9, 6.5p5, 6.5.5p5-6, 6.5.7p3-5, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
