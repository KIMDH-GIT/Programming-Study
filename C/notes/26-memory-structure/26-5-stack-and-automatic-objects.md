# 26-5. stack과 자동 객체
## 1. 학습 목표
- automatic storage duration을 stack과 구분한다.
- local objects와 parameter objects를 C17 관점에서 설명한다.
- automatic object lifetime 종료 후 dangling pointer를 분석한다.
## 2. 선수 지식
26-1 storage duration과 Part 10 function scope를 안다.
## 3. 핵심 개념
block-scope ordinary local object는 보통 automatic storage duration을 가진다.

```c
int add_one(int parameter)
{
    int local = parameter + 1;
    return local;
}
```

전형적인 implementation은 stack frame을 사용할 수 있지만 compiler는 register allocation, constant propagation, inlining, object 제거를 할 수 있다. 따라서 `automatic storage duration = stack storage`가 아니다.
## 4. 문법
```c
void f(void)
{
    int value = 10;
}
```

`value`의 block scope와 automatic storage duration은 language semantics다. 구체적 stack offset은 구현 결과다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int add_one(int parameter)
{
    int local = parameter + 1;

    return local;
}

int main(void)
{
    const int result = add_one(9);

    printf("%d\n", result);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o automatic_app
./automatic_app
```
## 6. 코드 해석
`parameter`, `local`, `result`는 automatic storage duration을 가진 objects다. program behavior는 10을 출력하는 것이며 어느 object가 machine stack에 실제 배치되는지는 요구하지 않는다.
## 7. 내부 동작
**[C17]** block entry·exit와 function call에 관련된 automatic storage duration과 lifetime을 규정한다.

**[compiler / linker]** stack frame, machine register, immediate value, elimination 중 적합한 representation을 선택할 수 있다.

**[OS / executable format]** process에 stack mapping과 size limit이 있을 수 있지만 이는 C17 규칙이 아니다.

**[CPU / ISA]** ABI가 stack pointer register와 calling convention을 정할 수 있다.

**[MIPS — 수업 기준]** `$sp` 사용은 ABI convention이다.

**[RISC-V — 병행 학습]** `sp` 사용 역시 ABI convention이며 C17이 강제하지 않는다.
## 8. 자주 하는 실수
- 모든 local variable이 반드시 stack에 있다고 말한다.
- parameter가 항상 stack slot에 전달된다고 말한다.
- recursion마다 C 표준이 물리적 stack frame을 요구한다고 말한다.
- stack은 항상 높은 주소에서 낮은 주소로 자란다고 일반화한다.
- local identifier scope와 object lifetime을 같은 개념으로 본다.

분석 전용 잘못된 예:
```c
int *bad(void)
{
    int value = 10;
    return &value;
}
```
return 뒤 `value` lifetime이 끝나므로 pointer를 dereference하면 안 된다. 실행 실습으로 만들지 않는다.
## 9. 필수 실습
automatic objects가 있는 function을 실행하고 source-level duration과 possible stack representation을 구분한다.
[26-5 exercise](../../exercises/26-memory-structure/26-5/README.md)
## 10. 추가 실습
- ★ local 계산을 하나 추가한다.
- ★★ optimization이 object storage를 없앨 수 있는 이유를 설명한다.
- ★★★ dangling pointer 분석에서 scope와 lifetime을 각각 표시한다.
## 11. 확인 문제
1. automatic storage duration은 stack을 뜻하는가?
2. compiler가 local object를 register에 둘 수 있는 이유는?
3. parameter object가 항상 stack에 있는가?
4. `bad`가 반환한 pointer가 위험한 근본 이유는?
5. stack growth direction은 C17 보장인가?
## 12. 핵심 정리
- automatic storage duration은 C17 semantics다.
- stack frame은 흔한 implementation strategy일 뿐이다.
- lifetime이 끝난 automatic object를 접근하지 않는다.
## 13. 다음 Step
[26-6. heap과 동적 객체](26-6-heap-and-dynamic-objects.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.4, 6.9.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Storage duration](https://en.cppreference.com/w/c/language/storage_duration)
- System V ABI calling convention documents — implementation reference
