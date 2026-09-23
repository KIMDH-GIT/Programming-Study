# 22-9. driver interface
## 1. 학습 목표
- function pointers를 구조체에 묶어 작은 driver interface를 만든다.
- interface의 function signatures를 구현과 정확히 맞춘다.
- object pointer와 function pointer 역할을 구분한다.
## 2. 선수 지식
Part 19 구조체, Part 20 typedef, 22-3 type compatibility를 안다.
## 3. 핵심 개념
driver interface는 caller가 특정 구현 이름을 직접 호출하는 대신, 약속된 function pointer members를 통해 동작을 요청하게 할 수 있다.
```c
struct LedDriver {
    void (*set)(unsigned int value);
    unsigned int (*get)(void);
};
```
`set`과 `get`은 function pointer members다. 구현 function은 각 member의 function type과 compatible해야 한다.
## 4. 문법
```c
struct LedDriver driver = {
    .set = mock_set,
    .get = mock_get
};
```
구조체 object는 function pointer values를 저장한다. function 자체가 구조체 안에 복사되는 것이 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static unsigned int led_state;

void mock_set(unsigned int value)
{
    led_state = value != 0u;
}

unsigned int mock_get(void)
{
    return led_state;
}

struct LedDriver {
    void (*set)(unsigned int value);
    unsigned int (*get)(void);
};

int main(void)
{
    struct LedDriver driver = {
        .set = mock_set,
        .get = mock_get
    };

    driver.set(1u);
    printf("%u\n", driver.get());
    return 0;
}
```
## 6. 코드 해석
`driver.set`과 `driver.get`은 각각 compatible mock function을 가리킨다. caller는 mock의 global state 구현 세부를 직접 다루지 않고 interface members를 호출한다.
## 7. 내부 동작
**[C17 type system]** 각 구조체 member는 pointer-to-function type의 object다. designated initializer의 expression은 compatible pointer value를 제공해야 한다.

**[compiler]** member access 뒤 function pointer call을 type-check하고 indirect call로 구현할 수 있다.

**[ABI]** 실제 embedded driver에서는 register access와 function call convention이 target 규칙을 따른다.

**[CPU / ISA]** interface call의 machine sequence는 optimization과 target에 따라 달라진다.
## 8. 자주 하는 실수
- driver interface에 function 자체가 저장된다고 설명한다.
- member signature와 구현 signature가 다른데 cast로 맞춘다.
- function pointer를 `void *`에 저장해 generic driver라고 설명한다. ISO C17의 `void *` object pointer conversion 보장은 function pointer에 그대로 적용되지 않는다.
- function pointer와 device state object의 lifetime을 같은 문제로 취급한다.
- 작은 고정 interface에 필요 없는 factory나 inheritance 흉내를 추가한다.
## 9. 필수 실습
set/get function pointer members를 가진 mock LED driver를 정의하고 두 operations를 호출한다.
[22-9 exercise](../../exercises/22-function-pointers/22-9/README.md)
## 10. 추가 실습
- ★ toggle member를 추가한다.
- ★★ 두 mock implementations 중 하나를 initializer에서 선택한다.
- ★★★ member 하나를 `NULL`로 둘 수 있는 optional contract를 설계하고 call 전 검사한다.
## 11. 확인 문제
1. 구조체에 저장되는 것은 function인가 function pointer value인가?
2. designated initializer에 필요한 compatibility는?
3. function pointer를 `void *`에 넣는 방식을 portable C17로 가르치면 안 되는 이유는?
4. driver member call은 반드시 특정 CPU instruction 하나가 되는가?
5. optional operation을 `NULL`로 표현하면 caller는 무엇을 해야 하는가?
## 12. 핵심 정리
- function pointer members로 구현 교체 가능한 작은 interface를 만들 수 있다.
- 모든 implementation은 member signature와 compatible해야 한다.
- object pointer와 function pointer conversion 규칙을 섞지 않는다.
## 13. 다음 Step
[22-10. 모의 GPIO driver와 callback](22-10-mock-gpio-driver-and-callback.md)
## 14. 참고 자료
- N1570 6.3.2.3, 6.5.2.2, 6.5.2.3, 6.7.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: struct declaration](https://en.cppreference.com/w/c/language/struct)
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
