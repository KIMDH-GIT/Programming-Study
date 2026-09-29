# 31-2. `volatile` access와 compiler optimization
## 1. 학습 목표
- volatile-qualified object access의 C17 의미를 설명한다.
- volatile access와 ordinary access의 optimization 경계를 구분한다.
- GCC의 구체적 동작을 ISO C 보장과 분리한다.
## 2. 선수 지식
31-1의 abstract machine, lvalue access, type qualifier를 안다.
## 3. 핵심 개념
**[C17 abstract machine]** volatile-qualified type의 object는 implementation이 알 수 없는 방식으로 변경될 수 있다. 무엇이 volatile access를 구성하는지는 implementation-defined이며, 표준의 최소 실행 요구와 sequence point 규칙을 따라야 한다.

`volatile`은 compiler가 해당 access를 일반적인 죽은 계산처럼 취급하지 못하게 하는 언어 계약이다. cache off, 항상 RAM 배치, hardware register 전용 type, 특정 CPU instruction 수를 뜻하지 않는다.

**[compiler-specific]** GCC는 scalar volatile lvalue의 void-context 평가를 read로 취급하는 등 구체적 규칙을 문서화한다. 이 설명은 GCC 계약이지 모든 C implementation의 동일한 code generation 보장이 아니다.
## 4. 문법
```c
volatile unsigned status;
unsigned snapshot = status;
status = snapshot + 1u;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    volatile unsigned status = 3u;
    unsigned snapshot = status;
    status = snapshot + 1u;
    printf("snapshot=%u status=%u\n", snapshot, status);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o volatile_access
./volatile_access
```
## 6. 코드 해석
현재 ordinary host object를 volatile-qualified로 선언한 안전한 예제이며 출력은 `snapshot=3 status=4`다. 실제 MMIO나 arbitrary address를 사용하지 않는다.
## 7. 내부 동작
**[C17 abstract machine]** 두 volatile access는 C expression의 평가다. access width, bus transaction, cache 정책은 C17만으로 정해지지 않는다.

**[compiler]** optimizer는 volatile access 자체의 의미를 지켜야 하지만 surrounding ordinary calculation은 여전히 최적화할 수 있다.

**[CPU / device]** device register access가 되려면 target compiler, ABI, address map, peripheral specification이 추가로 필요하다.
## 8. 자주 하는 실수
- volatile object는 항상 RAM에 존재한다고 말한다.
- `volatile uint32_t`가 특정 bus transaction 하나를 보장한다고 단정한다.
- volatile access 주변의 모든 code가 optimization되지 않는다고 생각한다.
- GCC 문서를 ISO C17의 모든 implementation 보장으로 일반화한다.
## 9. 필수 실습
ordinary volatile object를 read/write하고 strict build로 정의된 실행을 확인한다. [31-2 exercise](../../exercises/31-system-embedded-c/31-2/README.md)
## 10. 추가 실습
- ★ 초기값을 바꾸어 snapshot과 최종 값을 관찰한다.
- ★★ `-O0 -S`, `-O2 -S`를 비교하되 현재 GCC 관찰로 기록한다.
- ★★★ volatile access와 ordinary 계산을 구분해 표시한다.
## 11. 확인 문제
1. volatile-qualified object가 필요한 이유는 무엇인가?
2. 무엇이 volatile access인지는 누가 정의하는가?
3. volatile이 cache off를 뜻하지 않는 이유는?
4. 특정 access width에 어떤 추가 규약이 필요한가?
5. GCC 문서와 ISO C 규칙을 왜 구분해야 하는가?
## 12. 핵심 정리
- volatile은 C qualifier와 access 계약이다.
- exact code generation과 device transaction은 implementation/device 층이다.
- real hardware 없이 ordinary object로 안전하게 의미를 관찰한다.
## 13. 다음 Step
[31-3. `volatile`이 보장하지 않는 atomicity·동기화](31-3-volatile-non-guarantees-atomicity-synchronization.md)
## 14. 참고 자료
- WG14 N1570 5.1.2.3, 6.7.3. N1570은 C11 공개 Committee Draft이며 관련 volatile 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, When is a Volatile Object Accessed?: https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html
