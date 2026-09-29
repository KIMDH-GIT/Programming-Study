# 31-5. 레지스터 read-modify-write와 hardware 규약
## 1. 학습 목표
- `REG |= MASK`의 conceptual read-modify-write를 설명한다.
- ordinary memory와 W1C 같은 device register 규약을 구분한다.
- pure value transformation으로 안전하게 mask logic을 검증한다.
## 2. 선수 지식
31-4의 MMIO 층, Part 21의 bit mask를 안다.
## 3. 핵심 개념
`reg |= mask`는 C 관점에서 이전 값을 읽고 OR 결과를 계산해 다시 저장하는 read-modify-write expression이다. exact instruction 수는 compiler와 ISA가 정한다.

device register에는 write-one-to-clear(W1C), read side effect, reserved bit, write-only field 같은 별도 규약이 있을 수 있다. 그런 register에 ordinary memory와 같은 RMW를 적용하면 읽은 상태 bit를 의도치 않게 다시 쓸 수 있다. 안전성은 datasheet contract를 따라 판단한다.
## 4. 문법
```c
updated = (old_value & ~field_mask) | (field_value & field_mask);
```
이 순수 계산은 device access와 분리해 검증할 수 있다.
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>

static uint32_t replace_field(uint32_t old_value,
                              uint32_t field_mask,
                              uint32_t field_value)
{
    return (old_value & ~field_mask) | (field_value & field_mask);
}

int main(void)
{
    uint32_t before = UINT32_C(0xA5);
    uint32_t after = replace_field(before, UINT32_C(0x0F), UINT32_C(0x03));
    printf("before=%02X after=%02X\n", (unsigned)before, (unsigned)after);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o register_rmw
./register_rmw
```
## 6. 코드 해석
출력은 `before=A5 after=A3`이다. low nibble만 3으로 바꾸고 high nibble은 보존한다. 실제 register read/write는 수행하지 않는다.
## 7. 내부 동작
**[C17 abstract machine]** unsigned bitwise 연산은 정의된 value transformation이다.

**[compiler / ISA]** expression이 한 instruction인지 여러 instruction인지는 보장되지 않는다.

**[device/SoC-specific]** W1C와 reserved-bit 정책은 register마다 다르다. device-specific 규약 없이 `|=`가 안전하다고 말할 수 없다.

**[MIPS — 수업 기준]**, **[RISC-V — 병행 학습]** 모두 source RMW가 특정 load/OR/store 개수로 고정되지 않는다.
## 8. 자주 하는 실수
- `|=`가 atomic instruction 하나라고 가정한다.
- W1C register를 ordinary memory처럼 읽고 다시 쓴다.
- reserved bit를 보존해야 하는지 datasheet 없이 결정한다.
- pure mask test가 실제 device access를 검증했다고 보고한다.
## 9. 필수 실습
pure helper로 지정 field만 바뀌고 나머지 bit가 보존되는지 확인한다. [31-5 exercise](../../exercises/31-system-embedded-c/31-5/README.md)
## 10. 추가 실습
- ★ low nibble 값을 여러 개 바꾼다.
- ★★ 두 개의 독립 field mask를 검증한다.
- ★★★ W1C register에서 RMW가 위험한 sequence를 글로 설명한다.
## 11. 확인 문제
1. read-modify-write의 세 단계는?
2. exact instruction 수를 C17이 정하지 않는 이유는?
3. W1C란 어떤 device contract인가?
4. reserved bit 처리에는 무엇이 필요한가?
5. pure helper가 실제 hardware를 검증하지 않는 이유는?
## 12. 핵심 정리
- RMW expression과 hardware transaction을 분리한다.
- device register는 datasheet 규약을 따라야 한다.
- mask logic은 pure function으로 먼저 검증한다.
## 13. 다음 Step
[31-6. 대상의 fixed-width integer 제공 여부](31-6-fixed-width-integer-availability.md)
## 14. 참고 자료
- WG14 N1570 6.5.10, 6.5.12, 6.5.16.2. N1570은 C11 공개 Committee Draft이며 bitwise/assignment 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, Optimize Options: https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html
