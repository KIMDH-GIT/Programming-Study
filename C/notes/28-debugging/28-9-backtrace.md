# 28-9. `backtrace`
## 1. 학습 목표
- `backtrace`로 현재 frame부터 caller chain을 읽는다.
- frame 번호, function, argument, source location을 해석한다.
- GDB stack frame을 C17이 요구하는 physical stack과 동일시하지 않는다.
## 2. 선수 지식
Part 10의 함수 호출과 28-6의 breakpoint를 안다.
## 3. 핵심 개념
**[GDB]** backtrace는 program이 현재 위치에 도달한 호출 경로를 frame별로 요약한다. `backtrace` 또는 `bt`를 사용하며 frame 0이 현재 실행 중인 innermost frame이다.

optimized build에서는 argument나 local이 `<optimized out>`으로 보이거나 inline 때문에 frame이 달라질 수 있다.
## 4. 문법
```gdb
break leaf
run
backtrace
bt
frame 1
info args
info locals
```

`frame`은 backtrace의 특정 frame을 선택한다. call stack·stack frame은 compiler·ABI·runtime 구현 모델이며 ISO C17이 물리 구조를 요구하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int leaf(int value)
{
    return value + 2;
}

static int middle(int value)
{
    return leaf(value * 2);
}

static int top(int value)
{
    return middle(value + 1);
}

int main(void)
{
    printf("%d\n", top(19));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o gdb_backtrace
gdb ./gdb_backtrace
```
## 6. 코드 해석
`top(19)` → `middle(20)` → `leaf(40)` 순서로 호출되어 결과 `42`를 출력한다. `leaf` breakpoint의 backtrace에서 이 caller chain을 확인할 수 있다.
## 7. 내부 동작
**[C17]** function call과 automatic object lifetime을 규정한다.

**[GCC / ABI]** generated call sequence와 debug frame information을 만든다.

**[GDB]** unwind information, registers, stack memory를 이용해 frames를 복원한다.

**[OS / CPU]** 현재 target의 register와 call convention이 관찰에 영향을 준다. source-level frame은 반드시 한 physical stack block과 일대일 대응하지 않는다.
## 8. 자주 하는 실수
- frame 0을 최초 caller라고 읽는다.
- library frame만 보고 user source frame을 놓친다.
- backtrace를 C17이 요구하는 physical stack이라고 한다.
- optimized-out argument를 object가 존재하지 않았다는 뜻으로 해석한다.
## 9. 필수 실습
`leaf`에 breakpoint를 걸고 `bt`, `frame 1`, `info args`로 호출 경로를 읽는다.
[28-9 exercise](../../exercises/28-debugging/28-9/README.md)
## 10. 추가 실습
- ★ 각 frame의 argument를 기록한다.
- ★★ `bt 2`와 전체 backtrace를 비교한다.
- ★★★ `-O2` build의 frame 차이를 관찰한다.
## 11. 확인 문제
1. frame 0은 무엇인가?
2. `bt`가 보여주는 핵심 정보는?
3. `frame 1`의 목적은?
4. optimized-out은 무엇을 뜻하는가?
5. call stack이 C17 필수 구조가 아닌 이유는?
## 12. 핵심 정리
- backtrace로 fault 또는 breakpoint까지의 caller chain을 찾는다.
- user frame과 argument를 중심으로 가설을 세운다.
- frame 모양은 compiler·ABI·optimization에 의존한다.
## 13. 다음 Step
[28-10. `watch`](28-10-watch.md)
## 14. 참고 자료
- [GDB: Backtraces](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Backtrace.html)
- [GDB: Selecting a Frame](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Selection.html)
