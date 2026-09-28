# 28-10. `watch`
## 1. 학습 목표
- watchpoint로 expression 값 변경 시점을 찾는다.
- code breakpoint와 data watchpoint를 구분한다.
- hardware·software watchpoint 차이를 구현 관찰로 설명한다.
## 2. 선수 지식
28-6의 breakpoint와 28-8의 variable inspection을 안다.
## 3. 핵심 개념
**[GDB]** `watch expression`은 expression 값이 program write로 변경될 때 execution을 멈추도록 요청한다. 변경 위치를 미리 모를 때 유용한 data breakpoint다.

GDB는 target이 지원하면 hardware watchpoint를, 그렇지 않으면 software watchpoint를 사용할 수 있다. resource 수와 정확한 구현은 target에 의존한다.
## 4. 문법
```gdb
break main
run
next 2
watch total
continue
print total
continue
info watchpoints
```

local automatic object가 scope를 벗어나면 해당 expression watchpoint는 삭제될 수 있다. constant address 숫자 자체는 변하지 않으므로 memory를 감시하려면 유효한 typed lvalue가 필요하다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[] = {1, 2, 3};
    int total = 0;

    for (size_t i = 0; i < sizeof values / sizeof values[0]; ++i) {
        total += values[i];
    }
    printf("%d\n", total);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o gdb_watch
gdb ./gdb_watch
```
## 6. 코드 해석
두 declaration을 실행한 뒤 watchpoint를 설정하면 `total`은 이미 0이고 이후 1, 3, 6으로 바뀐다. watchpoint는 각 실제 변경을 현재 target이 지원하는 방식으로 관찰하고 최종 output은 `6`이다.
## 7. 내부 동작
**[C17]** assignment와 object lifetime을 규정한다.

**[GDB]** watched expression을 평가하고 변경을 감지한다.

**[CPU / target]** hardware debug registers를 사용할 수 있으며 크기·개수 제한이 있다.

**[software watchpoint]** single-step과 반복 evaluation으로 구현될 수 있어 느리고 stop 위치가 다를 수 있다.
## 8. 자주 하는 실수
- watchpoint를 특정 source line breakpoint와 같다고 한다.
- `watch 0x1234`가 그 address의 memory를 감시한다고 생각한다.
- hardware watchpoint가 모든 target에서 무제한이라고 생각한다.
- scope가 끝난 local을 계속 valid object로 본다.
## 9. 필수 실습
`total` initialization 뒤 watchpoint를 설정하고 세 변경과 최종 값을 기록한다.
[28-10 exercise](../../exercises/28-debugging/28-10/README.md)
## 10. 추가 실습
- ★ `info watchpoints`를 확인한다.
- ★★ array 한 원소의 변경을 감시한다.
- ★★★ hardware와 software watchpoint 차이를 조사한다.
## 11. 확인 문제
1. watchpoint는 언제 멈추는가?
2. breakpoint와 어떤 점이 다른가?
3. hardware watchpoint의 제한은 어디서 오는가?
4. local variable scope 종료가 미치는 영향은?
5. constant address를 직접 watch할 수 없는 이유는?
## 12. 핵심 정리
- 변경 위치를 모를 때 watchpoint로 write 시점을 찾는다.
- expression의 scope·lifetime을 확인한다.
- hardware/software 방식은 GDB와 target 구현 영역이다.
## 13. 다음 Step
[28-11. 결함 재현·수정·재검증](28-11-reproduce-fix-retest.md)
## 14. 참고 자료
- [GDB: Setting Watchpoints](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Set-Watchpoints.html)
