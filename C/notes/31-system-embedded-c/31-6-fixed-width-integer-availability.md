# 31-6. 대상의 fixed-width integer 제공 여부
## 1. 학습 목표
- exact-width integer type이 optional인 이유를 설명한다.
- `uint32_t`가 존재할 때의 정확한 의미를 안다.
- C type width와 CPU register width를 구분한다.
## 2. 선수 지식
Part 4의 `<stdint.h>`, `CHAR_BIT`, unsigned integer를 안다.
## 3. 핵심 개념
`int32_t`, `uint32_t` 같은 exact-width type은 implementation이 padding 없는 정확한 폭의 integer type을 제공할 수 있을 때 정의된다. 모든 implementation에 반드시 존재하는 type이 아니다.

`uint32_t`가 정의되어 있다면 정확히 32 value bit를 가진 unsigned integer type이다. 이는 CPU register 하나에 저장되거나 bus access가 32 bit 하나라는 보장이 아니다. `<stdint.h>`의 `UINT32_MAX` macro로 제공 여부를 조건부 확인할 수 있다.
## 4. 문법
```c
#include <stdint.h>

#ifdef UINT32_MAX
uint32_t value = UINT32_C(1);
#endif
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>

int main(void)
{
#ifdef UINT32_MAX
    uint32_t value = UINT32_C(0xFFFFFFFF);
    printf("uint32_t=available size=%zu max_match=%d\n",
           sizeof value, value == UINT32_MAX);
#else
    puts("uint32_t=unavailable");
#endif
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o fixed_width
./fixed_width
```
## 6. 코드 해석
현재 host에서는 `uint32_t=available size=4 max_match=1`을 관찰한다. 다른 conforming implementation에서는 `uint32_t=unavailable`도 허용된다. `sizeof value == 4`는 현재 host observation이며 C byte가 항상 8 bit라는 일반 증명이 아니다.
## 7. 내부 동작
**[ISO C17 portable]** exact-width typedef는 제공되는 경우에만 사용한다. `uint_least32_t`와 `uint_fast32_t`는 별도 요구와 목적을 가진다.

**[ABI / CPU]** type의 object representation, argument passing, register allocation은 ABI/compiler 문제다.

`uint8_t`도 존재한다면 정확히 8 bit이며 흔히 `unsigned char`의 typedef일 수 있다. 반드시 별도 type이라고 말하지 않는다.
## 8. 자주 하는 실수
- 모든 C implementation에 `uint32_t`가 있다고 가정한다.
- `sizeof(uint32_t) == 4`만 보고 C byte가 항상 8 bit라고 결론낸다.
- 32-bit C type이 CPU register 하나를 보장한다고 말한다.
- `uint8_t`가 반드시 `unsigned char`와 다른 type이라고 말한다.
## 9. 필수 실습
macro로 제공 여부를 분기하고 현재 host observation을 기록한다. [31-6 exercise](../../exercises/31-system-embedded-c/31-6/README.md)
## 10. 추가 실습
- ★ `UINT16_MAX` 제공 여부도 확인한다.
- ★★ `CHAR_BIT`와 `sizeof(uint32_t) * CHAR_BIT`를 관찰한다.
- ★★★ exact/least/fast type의 계약 차이를 정리한다.
## 11. 확인 문제
1. `uint32_t`는 모든 implementation에서 필수인가?
2. 존재하는 `uint32_t`는 무엇을 보장하는가?
3. `uint32_t`가 존재하는 implementation에서 `sizeof(uint32_t) == 4`는 `CHAR_BIT == 8`을 뜻한다. 그런데 이 결과를 모든 implementation의 byte 폭 보장으로 일반화할 수 없는 이유는?
4. C type width와 register width가 다른 층인 이유는?
5. `uint8_t`가 typedef일 수 있다는 뜻은?
## 12. 핵심 정리
- exact-width type은 optional이다.
- 존재하면 정확한 폭과 padding 부재를 보장한다.
- C type, ABI, CPU register, bus width를 분리한다.
## 13. 다음 Step
[31-7. bit mask·promotion·shift 경계](31-7-bit-mask-promotion-and-shift-boundaries.md)
## 14. 참고 자료
- WG14 N1570 5.2.4.2.1, 7.20.1.1. N1570은 C11 공개 Committee Draft이며 exact-width type 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- cppreference, fixed width integer types: https://en.cppreference.com/w/c/types/integer.html
