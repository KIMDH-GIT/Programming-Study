# 31-15. Part 31 종합 복습
## 1. 학습 목표
- C17, compiler, ABI, OS/runtime, CPU/device 층을 다시 구분한다.
- volatile, MMIO mock, mask, byte order, layout, interface contract를 연결한다.
- unsafe hardware/UB 실행 없이 Part 31 범위를 검증한다.
## 2. 선수 지식
31-1부터 31-14까지의 abstract machine, volatile, MMIO, representation, driver interface를 안다.
## 3. 핵심 개념
Part 31의 핵심은 source syntax를 hardware fact로 곧바로 등치하지 않는 것이다.

- **[C17 abstract machine]** object, value, lifetime, access, observable behavior
- **[compiler]** optimization, code generation, diagnostic, extension
- **[ABI]** calling convention, alignment/layout convention
- **[OS / runtime]** hosted process와 virtual address environment
- **[CPU / device]** instruction, register, cache, bus, peripheral contract

실제 dependency가 있는 층과 필요한 assumption을 기록해야 한다.

전체 review에서는 abstract machine과 `volatile`부터 MMIO safe mock, RMW/device contract, fixed-width integer 제공 여부, shift 경계, endianness, alignment·padding, pointer casting·aliasing, const interface, function pointer compatibility, callback ownership·lifetime까지 각 계약을 연결한다.
## 4. 문법
```c
typedef void (*event_fn)(void *context, unsigned value);
```
function pointer compatibility, context lifetime, const/volatile access를 interface contract로 묶는다.
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>

typedef struct {
    volatile uint32_t control;
    volatile uint32_t status;
} MockDevice;

typedef void (*event_fn)(void *context, uint32_t value);

typedef struct {
    uint32_t last;
} EventContext;

static uint32_t read_le16(const unsigned char bytes[2])
{
    return (uint32_t)bytes[0] | ((uint32_t)bytes[1] << 8);
}

static void remember(void *context, uint32_t value)
{
    EventContext *event = context;
    event->last = value;
}

int main(void)
{
    MockDevice mock = {0u, 5u};
    EventContext context = {0u};
    const unsigned char command[2] = {0x34u, 0x12u};
    uint32_t value = read_le16(command);

    mock.control = value & UINT32_C(0xFF);
    event_fn callback = remember;
    callback(&context, mock.status);
    printf("control=%u event=%u\n",
           (unsigned)mock.control, (unsigned)context.last);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part31_review
./part31_review
```
## 6. 코드 해석
출력은 `control=52 event=5`다. explicit little-endian 조립, unsigned mask, ordinary mock device, compatible callback, live context만 사용한다.
## 7. 내부 동작
**[ISO C17 portable]** valid object와 unsigned operation, compatible callback call을 실행한다.

**[implementation/compiler-specific]** volatile access의 exact code generation과 concrete layout은 별도 관찰이다.

**[device/SoC-specific]** 실제 address, RMW/W1C, access width, ordering은 검증했다고 주장하지 않는다.

**[MIPS — 수업 기준]**, **[RISC-V — 병행 학습]** 어느 쪽도 C assignment/dereference를 instruction 하나로 고정하지 않는다.
## 8. 자주 하는 실수
- volatile을 atomic, barrier, cache off, RAM 배치로 설명한다.
- pointer를 physical address로 일반화한다.
- host endianness/layout을 C17 보장으로 말한다.
- incompatible callback cast와 expired context를 사용한다.
- mock 성공을 real hardware 성공으로 보고한다.
## 9. 필수 실습
15개 Step의 portability layer, assumption, safe validation을 표로 정리한다. [31-15 exercise](../../exercises/31-system-embedded-c/31-15/README.md)
## 10. 추가 실습
- ★ 각 operation의 layer label을 붙인다.
- ★★ host-dependent observation을 portable invariant로 바꾼다.
- ★★★ device datasheet가 추가될 때 필요한 검증 목록을 작성한다.
## 11. 확인 문제
1. abstract machine과 CPU execution을 왜 구분하는가?
2. volatile이 보장하지 않는 네 가지는?
3. MMIO mock이 검증하지 않는 것은?
4. endianness와 alignment는 어떤 층에 의존하는가?
5. compatible function pointer type이 필요한 이유는?
6. callback context lifetime contract는 무엇인가?
7. unsafe hardware access를 실행하지 않은 이유는?
## 12. 핵심 정리
- C17 의미와 implementation/hardware 사실을 층별로 구분한다.
- safe mock과 portable value operation만 host에서 실행한다.
- 실제 device 검증은 target 문서와 hardware가 있을 때 별도로 수행한다.
## 13. 다음 Step
P4-1. 직접 사상 cache의 주소 폭·용량·block 크기
## 14. 참고 자료
- WG14 N1570 5.1.2.3, 6.2.4, 6.3.2.3, 6.5, 6.7.3, 7.20. N1570은 C11 공개 Committee Draft이며 사용한 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, Volatiles: https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html
- GCC, C17 language mode: https://gcc.gnu.org/onlinedocs/gcc/Standards.html
