# 21-7. 폭에 맞는 unsigned bit mask
## 1. 학습 목표
- unsigned type width를 계산한다.
- bit position을 검사한 뒤 mask를 만든다.
- fixed-width type의 제공 조건을 유지한다.
## 2. 선수 지식
21-5 shift count와 `CHAR_BIT`을 안다.
## 3. 핵심 개념
mask는 특정 bit positions를 선택하는 pattern이다. `1u << position`은 `position`이 promoted unsigned int의 value bits 수보다 작을 때만 사용한다.
## 4. 문법
```c
unsigned int width = uint_value_bits();
if (position < width) mask = 1u << position;
```
`uint32_t`는 `<stdint.h>`가 제공하는 implementation에서만 존재한다.
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
    unsigned int position = 5u;
    unsigned int width = uint_value_bits();
    if (position >= width) return 1;
    unsigned int mask = 1u << position;
    printf("%X %u\n", mask, width);
    return 0;
}
```
## 6. 코드 해석
`UINT_MAX`를 right shift하여 unsigned int value bits를 세고 valid position에서 mask를 만든다.
## 7. 내부 동작
**[C17 표준]** `sizeof(T) * CHAR_BIT`은 padding을 포함할 수 있는 object representation bits다. `UINT_MAX`를 0까지 right shift해 센 값은 unsigned int value bits이므로 shift-count guard에 사용한다.
## 8. 자주 하는 실수
- unsigned int를 32 bits로 고정한다.
- `1 << position`으로 sign bit UB를 만든다.
- position을 width로 modulo 처리될 것이라 가정한다.
- bit position과 memory byte order를 같은 개념으로 본다.
## 9. 필수 실습
position validation 뒤 single-bit unsigned mask를 출력한다.
[21-7 exercise](../../exercises/21-bitwise-operators/21-7/README.md)
## 10. 추가 실습
- ★ 여러 positions를 출력한다.
- ★★ invalid position을 거부한다.
- ★★★ value bits와 padding bits 차이를 조사한다.
## 11. 확인 문제
1. width 계산식은?
2. position의 valid 범위는?
3. `1u`를 쓰는 이유는?
4. bit position과 endianness는 같은가?
## 12. 핵심 정리
- type width를 계산하고 position을 검사한다.
- unsigned single-bit mask를 사용한다.
- mask는 abstract integer value에 적용된다.
## 13. 다음 Step
[21-8. bit set](21-8-bit-set.md)
## 14. 참고 자료
- N1570 5.2.4.2.1, 6.2.6.2, 6.5.7. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: numeric limits](https://en.cppreference.com/w/c/types/limits)
