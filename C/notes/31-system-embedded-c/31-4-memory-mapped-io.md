# 31-4. memory-mapped I/O
## 1. 학습 목표
- MMIO를 C expression부터 peripheral까지 층별로 설명한다.
- integer-to-pointer conversion의 portability 경계를 안다.
- host에서는 safe mock만 실행하고 arbitrary address를 역참조하지 않는다.
## 2. 선수 지식
31-2의 volatile access, Part 14의 pointer, Part 19의 struct를 안다.
## 3. 핵심 개념
MMIO는 CPU의 address space에 device register가 배치되는 hardware/implementation 기법이다. C expression에서 device effect까지는 다음 계약이 필요하다.

`C lvalue access -> implementation-defined address/pointer mapping -> compiler load/store -> CPU memory access -> bus/interconnect -> peripheral register`

integer를 pointer로 변환한 결과는 implementation-defined이고 올바르게 정렬되거나 유효한 object를 가리킨다는 portable 보장이 없다. `uintptr_t`도 `<stdint.h>`의 optional type이므로 항상 존재한다고 가정하지 않는다.
## 4. 문법
```c
typedef struct {
    volatile unsigned control;
    volatile unsigned status;
} MockDevice;
```
실제 address macro와 cast는 board/compiler/device 문서가 있을 때만 사용한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef struct {
    volatile unsigned control;
    volatile unsigned status;
} MockDevice;

static void start(MockDevice *device)
{
    device->control = 1u;
}

int main(void)
{
    MockDevice mock = {0u, 7u};
    start(&mock);
    printf("control=%u status=%u\n", mock.control, mock.status);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o mmio_mock
./mmio_mock
```
## 6. 코드 해석
출력은 `control=1 status=7`이다. `mock`은 ordinary C object이며 실제 peripheral이나 physical address가 아니다.
## 7. 내부 동작
**[ISO C17 portable]** object access와 qualifier 규칙만 다룬다.

**[implementation-defined]** integer-to-pointer conversion 결과와 device address 사용 가능성은 implementation 문서가 필요하다.

**[CPU / device]** access width, side effect, ordering, reset value는 SoC와 peripheral specification이 정한다.

`(volatile unsigned *)0x40000000` 같은 주소는 개념 예시일 뿐 실제 device address가 아니며 host에서 실행하지 않는다.
## 8. 자주 하는 실수
- pointer를 physical RAM address라고 일반화한다.
- toy address를 host에서 역참조한다.
- volatile pointer만으로 device access width와 ordering이 보장된다고 말한다.
- `uintptr_t`와 exact-width integer type이 항상 있다고 가정한다.
## 9. 필수 실습
safe mock object로 control write와 status read를 수행하고 실제 MMIO가 아닌 이유를 설명한다. [31-4 exercise](../../exercises/31-system-embedded-c/31-4/README.md)
## 10. 추가 실습
- ★ mock field를 하나 더 추가한다.
- ★★ C/implementation/compiler/CPU/device 층을 표로 나눈다.
- ★★★ 실제 MCU datasheet가 있다면 주소와 register 규약을 실행 없이 분석한다.
## 11. 확인 문제
1. MMIO는 C17만의 개념인가?
2. integer-to-pointer conversion에서 무엇이 implementation-defined인가?
3. volatile이 bus transaction 하나를 보장하지 않는 이유는?
4. toy address를 host에서 실행하면 안 되는 이유는?
5. safe mock이 검증하는 것과 검증하지 않는 것은?
## 12. 핵심 정리
- MMIO는 C, compiler, CPU, bus, device 계약의 결합이다.
- arbitrary address를 host에서 실행하지 않는다.
- 실제 hardware 없이 mock으로 C interface만 검증한다.
## 13. 다음 Step
[31-5. 레지스터 read-modify-write와 hardware 규약](31-5-register-read-modify-write-and-hardware-contracts.md)
## 14. 참고 자료
- WG14 N1570 6.3.2.3, 6.7.3, 7.20.1.4. N1570은 C11 공개 Committee Draft이며 관련 pointer/volatile 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, Volatiles: https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html
