# 22-7. state machine과 함수 포인터
## 1. 학습 목표
- enum state를 function pointer table index로 안전하게 사용한다.
- state range를 검사한 뒤 handler를 dispatch한다.
- `switch`와 table 중 상황에 맞는 방식을 선택한다.
## 2. 선수 지식
Part 20 enum·state machine과 22-6 function pointer array를 안다.
## 3. 핵심 개념
enum values가 0부터 연속이고 마지막에 count sentinel을 둔 정의라면 function pointer table index로 사용할 수 있다.
```c
enum State {
    STATE_IDLE,
    STATE_RUNNING,
    STATE_STOPPED,
    STATE_COUNT
};
```
이 definition에서는 세 state enumerator가 0, 1, 2로 연속이고 `STATE_COUNT`가 element count 역할을 한다. 모든 enum이 자동으로 이런 전제를 만족하는 것은 아니다.
## 4. 문법
```c
typedef void (*StateHandler)(void);
StateHandler handlers[STATE_COUNT] = {
    handle_idle,
    handle_running,
    handle_stopped
};
```
외부 integer를 enum으로 cast했다고 유효한 state가 되는 것은 아니다. dispatch 전에 range를 검사한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

enum State {
    STATE_IDLE,
    STATE_RUNNING,
    STATE_STOPPED,
    STATE_COUNT
};

void handle_idle(void) { puts("idle"); }
void handle_running(void) { puts("running"); }
void handle_stopped(void) { puts("stopped"); }

int main(void)
{
    void (*handlers[STATE_COUNT])(void) = {
        handle_idle,
        handle_running,
        handle_stopped
    };
    enum State state = STATE_RUNNING;

    if (state >= STATE_IDLE && state < STATE_COUNT) {
        handlers[state]();
    }
    return 0;
}
```
## 6. 코드 해석
enum definition이 연속 index 전제를 직접 만든다. range check가 성공하면 대응하는 handler pointer를 선택해 호출한다.
## 7. 내부 동작
**[C17 type system]** enum은 integer type이다. 각 handler의 function type은 `void (void)`와 compatible하고, initializer에서 function designator가 변환된 pointer value는 `void (*)(void)` element type과 compatible하다.

**[compiler]** `switch`는 branch sequence나 jump table로, function pointer table은 indirect call로 구현될 수 있다. optimizer 선택은 source syntax와 일대일로 고정되지 않는다.

**[ABI]** handler pointer 표현과 call convention은 target ABI를 따른다.

**[CPU / ISA]** table dispatch는 indirect control flow와 연결될 수 있다.

**[MIPS — 수업 기준]** handler pointer를 통한 call이 `jalr` 같은 instruction과 연결될 수 있지만 C source가 항상 특정 sequence를 강제하지 않는다.

**[RISC-V — 병행 학습]** RISC-V에서도 `jalr`가 indirect call에 사용될 수 있으나 compiler·ABI가 실제 code sequence를 정한다.
## 8. 자주 하는 실수
- 모든 enum values가 항상 0부터 연속이라고 가정한다.
- `STATE_COUNT`를 실제 state처럼 dispatch한다.
- range check 없이 table을 indexing한다.
- function pointer table이 `switch`보다 언제나 더 좋거나 빠르다고 단정한다.
- handler prototype을 `void handler()`처럼 old-style로 작성한다. `void handler(void)`를 사용한다.
## 9. 필수 실습
세 state와 count sentinel을 정의하고 bounds 검사 뒤 handler table을 호출한다.
[22-7 exercise](../../exercises/22-function-pointers/22-7/README.md)
## 10. 추가 실습
- ★ 새로운 PAUSED state와 handler를 추가한다.
- ★★ invalid integer input을 거부한다.
- ★★★ 같은 state machine을 `switch`로 작성해 가독성을 비교한다.
## 11. 확인 문제
1. enum을 table index로 쓰려면 어떤 전제가 필요한가?
2. `STATE_COUNT`의 역할은?
3. range check가 필요한 이유는?
4. `switch`와 function pointer table 중 하나가 항상 우월한가?
5. no-parameter handler에 `(void)`를 쓰는 이유는?
## 12. 핵심 정리
- enum definition에서 연속 index 전제를 명시한다.
- count sentinel과 bounds check로 table access를 보호한다.
- table dispatch와 `switch`는 요구사항에 따라 선택한다.
## 13. 다음 Step
[22-8. interrupt handler와 일반 callback의 차이](22-8-interrupt-handler-vs-callback.md)
## 14. 참고 자료
- N1570 6.2.5, 6.7.2.2, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: enum declaration](https://en.cppreference.com/w/c/language/enum)
- [cppreference: function declaration](https://en.cppreference.com/w/c/language/function_declaration)
