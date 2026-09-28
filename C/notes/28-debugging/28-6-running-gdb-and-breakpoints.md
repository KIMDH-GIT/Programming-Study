# 28-6. GDB 실행과 breakpoint
## 1. 학습 목표
- debug build를 GDB에서 시작한다.
- function·source line breakpoint를 설정하고 `run`으로 inferior를 실행한다.
- GDB breakpoint와 C `break` statement를 구분한다.
## 2. 선수 지식
28-3의 `-g -Og` build와 함수 호출을 안다.
## 3. 핵심 개념
**[GDB]** GDB는 GNU debugger이며 ISO C17의 일부가 아니다. breakpoint는 program이 특정 execution point에 도달할 때 debugger가 멈추게 하는 mechanism이다.

C `break;`는 loop나 `switch`의 control statement다. 두 기능은 이름만 비슷하다.
## 4. 문법
```sh
gdb ./gdb_break
```

```gdb
break double_value
run
print value
continue
quit
```

`run`은 inferior program을 시작한다. C syntax로 `main`을 수동 호출하는 명령이 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int double_value(int value)
{
    int result = value * 2;
    return result;
}

int main(void)
{
    int answer = double_value(21);

    printf("%d\n", answer);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o gdb_break
gdb ./gdb_break
```
## 6. 코드 해석
`break double_value` 뒤 `run`하면 첫 stop이 함수 entry의 source 위치다. `print value`로 현재 argument 21을 관찰하고 `continue`하면 `42`가 출력된다. `break main`이나 line breakpoint도 별도 실행에서 연습할 수 있지만, 여러 breakpoint를 동시에 두면 먼저 도달한 위치에서 멈춘다.
## 7. 내부 동작
**[GDB]** symbol과 line information을 읽고 breakpoint를 target에 설치한다.

**[GCC]** debug information과 generated instructions의 source mapping을 제공한다.

**[OS / ABI / CPU]** breakpoint 구현은 software trap 또는 target 지원에 의존할 수 있다. source 한 줄이 정확히 instruction 하나라는 뜻은 아니다.

**[C17]** function call과 arithmetic 의미만 규정하며 breakpoint를 모른다.
## 8. 자주 하는 실수
- GDB를 C language feature라고 한다.
- breakpoint와 C `break`를 같은 기능으로 본다.
- `run`이 C 함수 호출 문법이라고 생각한다.
- 한 breakpoint 관찰로 모든 input의 correctness를 증명한다.
## 9. 필수 실습
`double_value`에 breakpoint를 걸고 argument를 읽은 뒤 execution을 계속한다.
[28-6 exercise](../../exercises/28-debugging/28-6/README.md)
## 10. 추가 실습
- ★ `break main`과 function breakpoint를 비교한다.
- ★★ source line breakpoint를 추가한다.
- ★★★ batch mode command와 interactive session을 비교한다.
## 11. 확인 문제
1. GDB는 어느 층의 도구인가?
2. breakpoint의 목적은?
3. C `break`와 무엇이 다른가?
4. `run`이 실행하는 대상은?
5. source line과 instruction이 일대일이 아닌 이유는?
## 12. 핵심 정리
- `-g -Og` executable을 GDB로 연다.
- breakpoint로 의심 위치에서 멈추고 state를 읽는다.
- debugger 관찰은 correctness proof가 아니라 evidence다.
## 13. 다음 Step
[28-7. `step`, `next`, `continue`](28-7-step-next-continue.md)
## 14. 참고 자료
- [GDB: Breakpoints](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Breakpoints.html)
- [GDB: Starting](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Starting.html)
