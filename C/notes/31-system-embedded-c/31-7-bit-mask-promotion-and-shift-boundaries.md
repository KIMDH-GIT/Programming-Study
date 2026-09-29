# 31-7. bit mask·promotion·shift 경계
## 1. 학습 목표
- integer promotion이 bit operation에 미치는 영향을 설명한다.
- shift count와 unsigned left-shift 경계를 검사한다.
- 안전한 field mask를 만들어 정의된 동작만 실행한다.
## 2. 선수 지식
Part 6의 conversion, Part 21의 bitwise operator, 31-6의 `uint32_t`를 안다.
## 3. 핵심 개념
작은 integer type도 expression에서 integer promotion을 거칠 수 있다. shift operator의 두 operand에도 integer promotion이 적용되며, 오른쪽 operand가 음수이거나 promoted left operand의 width 이상이면 behavior가 undefined다.

unsigned left shift는 결과가 해당 unsigned type에서 표현 가능한 범위의 modulo arithmetic으로 정의되지만, shift count 자체의 경계는 먼저 검사해야 한다. promoted left operand가 signed type이면 값이 nonnegative이고 그 값에 `2`의 shift count 제곱을 곱한 수가 결과 type으로 표현 가능할 때만 그 수가 결과가 되며, 그렇지 않으면 behavior가 undefined다. signed value의 shift에 hardware 관찰을 근거로 의존하지 않는다.
## 4. 문법
```c
static int make_mask(unsigned width, unsigned shift, uint32_t *out);
```
`out`은 non-NULL writable object이고 성공 시에만 갱신한다.
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>

static int make_mask(unsigned width, unsigned shift, uint32_t *out)
{
    if (out == NULL || width == 0u || width > 32u ||
        shift >= 32u || width + shift > 32u) {
        return 0;
    }
    uint32_t low = UINT32_MAX >> (32u - width);
    *out = low << shift;
    return 1;
}

int main(void)
{
    uint32_t mask = 0u;
    int ok = make_mask(4u, 8u, &mask);
    int rejected = !make_mask(1u, 32u, &mask);
    printf("ok=%d mask=%08X rejected=%d\n",
           ok, (unsigned)mask, rejected);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bit_mask
./bit_mask
```
## 6. 코드 해석
출력은 `ok=1 mask=00000F00 rejected=1`이다. invalid shift는 expression을 평가하기 전에 거부하므로 UB를 실행하지 않는다.
## 7. 내부 동작
`UINT32_MAX >> (32 - width)`는 width가 1~32일 때만 평가된다. width가 32면 shift count는 0이다. 만들어진 unsigned mask를 다시 0~31 범위에서 left shift한다.

**[compiler / CPU]** hardware가 shift count를 자동 masking하더라도 C의 invalid shift가 정의되는 것은 아니다.
## 8. 자주 하는 실수
- `1 << 31`에서 literal `1`의 signed type을 무시한다.
- invalid shift를 실행한 뒤 결과를 검사한다.
- `uint8_t` operand가 promotion되지 않는다고 생각한다.
- CPU가 count를 masking하므로 C에서도 안전하다고 주장한다.
## 9. 필수 실습
정상 field와 width/shift 경계 실패를 모두 확인한다. [31-7 exercise](../../exercises/31-system-embedded-c/31-7/README.md)
## 10. 추가 실습
- ★ width 1과 32를 검사한다.
- ★★ 여러 field가 겹치는지 mask AND로 확인한다.
- ★★★ signed와 unsigned literal 차이를 compiler diagnostic과 함께 설명한다.
## 11. 확인 문제
1. integer promotion은 언제 적용되는가?
2. shift count가 invalid한 두 경우는?
3. hardware shift 결과로 C behavior를 정할 수 없는 이유는?
4. invalid input을 shift 전에 거부해야 하는 이유는?
5. unsigned mask가 유리한 이유는?
## 12. 핵심 정리
- promotion 뒤 type과 width를 기준으로 shift를 판단한다.
- shift count를 먼저 검증한다.
- system code에서도 UB를 hardware behavior로 정당화하지 않는다.
## 13. 다음 Step
[31-8. endianness와 명시적 byte 조립](31-8-endianness-and-explicit-byte-assembly.md)
## 14. 참고 자료
- WG14 N1570 6.3.1.1, 6.5.7. N1570은 C11 공개 Committee Draft이며 promotion/shift 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
