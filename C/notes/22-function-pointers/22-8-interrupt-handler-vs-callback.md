# 22-8. interrupt handler와 일반 callback의 차이
## 1. 학습 목표
- ISO C17의 일반 callback과 platform interrupt handler를 구분한다.
- interrupt 등록·호출 규약이 C17 표준 밖의 hardware·ABI contract임을 설명한다.
- 일반 callback 예제에 compiler extension을 섞지 않는다.
## 2. 선수 지식
22-5 callback과 시스템의 기본 control flow를 이해한다.
## 3. 핵심 개념
일반 callback은 C code가 compatible function pointer를 전달하고 정상적인 function call semantics로 호출한다.

interrupt handler는 hardware event에 따라 control이 옮겨지는 platform 개념이다. handler의 등록 방식, required signature, 저장해야 할 machine state, return sequence, 실행 제약은 MCU 문서·compiler·ABI가 정한다. ISO C17에는 보편적인 `interrupt` keyword나 interrupt handler type이 없다.
## 4. 문법
portable C17 일반 callback은 다음처럼 쓸 수 있다.
```c
typedef void (*EventCallback)(void);

void notify(EventCallback callback)
{
    if (callback != NULL) {
        callback();
    }
}
```
`__attribute__((interrupt))`, vendor keyword, vector table 배치 등은 implementation-specific이므로 C17 최소 예제에 넣지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

typedef void (*EventCallback)(void);

void on_event(void)
{
    puts("event");
}

void notify(EventCallback callback)
{
    if (callback != NULL) {
        callback();
    }
}

int main(void)
{
    notify(on_event);
    return 0;
}
```
## 6. 코드 해석
`main`이 `on_event`의 function pointer value를 `notify`에 전달한다. `notify`는 null check 뒤 일반 C function call로 callback을 실행한다. 이 예제는 interrupt를 흉내 내거나 실제 ISR이라고 주장하지 않는다.
## 7. 내부 동작
**[C17 type system]** `EventCallback`은 `void (*)(void)` type의 typedef name이고 call은 일반 function pointer call이다.

**[compiler]** 일반 callback은 C calling convention에 맞는 call로 번역한다. vendor interrupt extension은 별도 prologue·epilogue를 만들 수 있다.

**[ABI]** interrupt handler ABI는 일반 function ABI와 다를 수 있다. compatible해 보이는 C declarator만으로 platform ISR contract 충족을 보장하지 않는다.

**[CPU / ISA]** interrupt entry는 CPU와 platform이 정의하며 일반 source-level call과 시작 경로가 다르다.
## 8. 자주 하는 실수
- 일반 callback function을 곧바로 hardware ISR이라고 부른다.
- ISO C17에 표준 interrupt keyword가 있다고 생각한다.
- GCC나 vendor attribute를 portable C17 문법으로 소개한다.
- 일반 callback과 ISR이 항상 같은 calling convention을 쓴다고 가정한다.
- ISR에서 가능한 작업과 호출 가능한 library function을 platform 문서 확인 없이 단정한다.
## 9. 필수 실습
nullable 일반 event callback을 등록·호출하고, 이 code가 ISR이 아닌 이유를 주석으로 설명한다.
[22-8 exercise](../../exercises/22-function-pointers/22-8/README.md)
## 10. 추가 실습
- ★ callback을 다른 compatible function으로 바꾼다.
- ★★ null callback일 때 호출하지 않음을 확인한다.
- ★★★ 사용하는 MCU 문서에서 ISR signature를 찾아 일반 callback과 표로 비교하되 code에는 넣지 않는다.
## 11. 확인 문제
1. ISO C17이 interrupt handler 등록 방식을 정의하는가?
2. 일반 callback의 호출자는 누구인가?
3. interrupt handler ABI는 누가 정하는가?
4. vendor interrupt attribute를 최소 C17 예제에서 제외하는 이유는?
5. C declarator가 같아 보이면 일반 callback과 ISR을 교환해도 되는가?
## 12. 핵심 정리
- 일반 callback은 C17 function pointer call이다.
- interrupt handler는 hardware·compiler·ABI contract가 추가된다.
- extension과 platform 규칙을 portable C17 설명과 분리한다.
## 13. 다음 Step
[22-9. driver interface](22-9-driver-interface.md)
## 14. 참고 자료
- N1570 5.1.2.3, 6.5.2.2, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 C abstract machine·call 규칙은 C17에서도 유지된다.
- [GCC: function attributes](https://gcc.gnu.org/onlinedocs/gcc/Common-Function-Attributes.html)
- 사용하는 target의 compiler·ABI·MCU interrupt documentation
