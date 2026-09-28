# 28-7. `step`, `next`, `continue`
## 1. 학습 목표
- `step`, `next`, `continue`의 실행 범위를 구분한다.
- function call 내부로 들어가거나 넘기는 방법을 선택한다.
- optimization과 debug information이 stepping 관찰에 미치는 영향을 안다.
## 2. 선수 지식
28-6의 GDB 실행·breakpoint를 안다.
## 3. 핵심 개념
**[GDB]** `step`은 다른 source line까지 진행하며 가능한 경우 현재 line에서 호출한 함수 내부로 들어간다. `next`는 현재 frame의 다음 source line까지 진행하며 line 안의 function call은 보통 멈추지 않고 수행한다. `continue`는 다음 breakpoint·signal·종료까지 execution을 재개한다.

GDB `continue`와 C `continue` statement는 서로 다른 기능이다.
## 4. 문법
```gdb
break main
run
step
next
continue
```

debug information이 없는 함수나 optimized code에서는 기대와 다른 source line 이동이 보일 수 있다. `finish`는 selected frame의 function이 return한 직후까지 진행하지만 이 Step의 필수 명령은 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int add(int left, int right)
{
    int result = left + right;
    return result;
}

int main(void)
{
    int answer = add(20, 22);

    printf("%d\n", answer);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o gdb_steps
gdb ./gdb_steps
```
## 6. 코드 해석
`main`의 `add` 호출 앞에서 `step`은 `add` 내부로 들어갈 수 있고, 같은 위치에서 새 실행을 시작해 `next`를 쓰면 호출을 넘겨 `answer == 42`인 다음 line으로 진행한다.
## 7. 내부 동작
**[GDB]** line table과 current frame을 사용해 source-level stepping을 구현한다.

**[GCC]** optimization이 instruction을 이동·제거·inline하면 source stepping 모양이 달라질 수 있다.

**[OS / CPU]** 실제로는 target execution을 제어한다. source “한 단계”는 machine instruction 한 개를 뜻하지 않는다.
## 8. 자주 하는 실수
- `step`과 `next`를 항상 동일하게 사용한다.
- GDB `continue`를 loop의 C `continue`로 설명한다.
- optimized build에서 line 이동이 다르면 C semantics가 바뀌었다고 한다.
- debugger에서 한 번 본 값만으로 모든 path를 판단한다.
## 9. 필수 실습
동일 program을 두 번 실행해 `step`과 `next`가 call에서 어떻게 다른지 비교한다.
[28-7 exercise](../../exercises/28-debugging/28-7/README.md)
## 10. 추가 실습
- ★ `continue`로 다음 breakpoint까지 이동한다.
- ★★ `finish`로 caller에 돌아온다.
- ★★★ `-Og`와 `-O2` stepping을 비교한다.
## 11. 확인 문제
1. `step`은 call을 어떻게 다루는가?
2. `next`는 현재 frame에서 어디까지 진행하는가?
3. `continue`는 언제 다시 멈추는가?
4. source step이 instruction 하나가 아닌 이유는?
5. optimization이 관찰에 미치는 영향은?
## 12. 핵심 정리
- call 내부 원인은 `step`, 호출 후 상태는 `next`로 관찰한다.
- `continue`로 다음 관찰 지점까지 실행한다.
- stepping 결과는 debug info와 optimization의 영향을 받는다.
## 13. 다음 Step
[28-8. 변수·메모리 출력](28-8-printing-variables-and-memory.md)
## 14. 참고 자료
- [GDB: Continuing and Stepping](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Continuing-and-Stepping.html)
