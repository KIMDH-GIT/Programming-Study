# 21-5. unsigned `<<`, `>>`
## 1. 학습 목표
- unsigned left/right shift를 설명한다.
- 유효 shift count를 검사한다.
- shift와 rotate·항상 빠른 곱셈을 구분한다.
## 2. 선수 지식
21-4 width 계산과 unsigned arithmetic을 안다.
## 3. 핵심 개념
unsigned left shift 결과는 `E1 * 2^E2`를 result type의 최대값보다 1 큰 수로 modulo한 값이다. unsigned right shift는 quotient의 integer part다. count가 음수거나 promoted left operand width 이상이면 UB다.
## 4. 문법
```c
unsigned int left = 1u << 3;
unsigned int right = 0x10u >> 2;
```
`1u`를 사용해 signed sign-bit 관련 문제를 피하지만 count 검사는 여전히 필요하다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
static unsigned int uint_value_bits(void)
{
    unsigned int bits = 0u;
    for (unsigned int value = UINT_MAX; value != 0u; value >>= 1) ++bits;
    return bits;
}
int main(void)
{
    unsigned int width = uint_value_bits();
    unsigned int count = 3u;
    if (count >= width) return 1;
    printf("%X %X\n", 1u << count, 0x20u >> count);
    return 0;
}
```
## 6. 코드 해석
`UINT_MAX`를 right shift해 unsigned int의 value bits 수를 구하고 그보다 작은 count만 사용한다. `sizeof * CHAR_BIT`은 padding까지 포함한 object representation bits일 수 있다.
## 7. 내부 동작
**[C17 표준]** operands에 integer promotions를 적용하고 count domain을 요구한다. unsigned rules는 정의되지만 shift가 rotate인 것은 아니다. **[compiler]** multiplication과 shift 중 target에 맞는 instructions를 선택하므로 shift가 항상 더 빠르다고 단정하지 않는다.
## 8. 자주 하는 실수
- count가 width 이상이면 자동 modulo된다고 생각한다.
- `1 << n`을 무조건 안전한 mask로 쓴다.
- `>>`를 모든 signed/unsigned 값에서 항상 `/2`라고 말한다.
- shift를 rotate라고 부른다.
## 9. 필수 실습
width를 계산하고 valid counts에서 unsigned left/right 결과를 출력한다.
[21-5 exercise](../../exercises/21-bitwise-operators/21-5/README.md)
## 10. 추가 실습
- ★ count 0을 확인한다.
- ★★ 여러 valid counts를 loop로 출력한다.
- ★★★ invalid counts를 실행하지 않고 분류한다.
## 11. 확인 문제
1. unsigned left shift의 수학적 규칙은?
2. unsigned right shift 결과는?
3. invalid count 조건은?
4. `1u`를 쓰는 이유는?
5. shift가 rotate 또는 항상 빠른 최적화인가?
## 12. 핵심 정리
- unsigned shifts도 count 경계를 지켜야 한다.
- width는 구현에서 계산한다.
- shift semantics와 optimization을 구분한다.
## 13. 다음 Step
[21-6. C17 signed shift의 UB·구현 정의 동작](21-6-c17-signed-shift-rules.md)
## 14. 참고 자료
- N1570 6.5.7. N1570은 **C11 공개 Committee Draft**이며 shift 규칙은 C17에서도 유지된다.
- [cppreference: shift operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
