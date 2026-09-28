# 28-12. Part 28 종합 복습
## 1. 학습 목표
- compiler diagnostics, GDB, sanitizer, tests의 역할을 종합한다.
- C17 semantics와 tool·OS·ISA observation을 일관되게 분리한다.
- 재현부터 regression 확인까지 evidence 기반 debugging을 수행한다.
## 2. 선수 지식
28-1부터 28-11까지를 학습했다.
## 3. 핵심 개념
```text
[C17]
syntax / constraints / semantics / behavior categories / object lifetime

[GCC]
dialect / diagnostics / debug information / optimization / instrumentation

[GDB]
breakpoint / stepping / state / frames / watchpoint

[Sanitizer]
instrumented runtime detection and report

[OS / ABI / CPU]
process / signal / executable / registers / frames / instructions
```

compiler diagnostics는 translation-time evidence, GDB는 live execution state, ASan은 instrumented memory-error detection, tests는 specified inputs의 behavior 검증이다. 어느 하나도 단독으로 모든 correctness를 증명하지 않는다.
## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o review_app
gdb ./review_app
```

```gdb
break find_value
run
print target
next
print i
backtrace
continue
```

ASan이 필요한 memory-error 사례만 별도 temporary build로 검증한다. UBSan은 `-fsanitize=undefined`로 일부 UB checks를 추가하는 별도 GCC instrumentation이며 clean run은 ISO C17 validation proof가 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int find_value(const int values[], size_t count, int target)
{
    for (size_t i = 0; i < count; ++i) {
        if (values[i] == target) {
            return (int)i;
        }
    }
    return -1;
}

int main(void)
{
    int values[] = {2, 4, 6, 8};
    int result = find_value(values, sizeof values / sizeof values[0], 6);

    printf("%d\n", result);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o part28_review
./part28_review
```
## 6. 코드 해석
valid bounds만 순회하고 target 6의 index `2`를 출력한다. GDB에서는 function argument, index, return path를 read-only로 관찰할 수 있다.
## 7. 내부 동작
**[C17]** array access, comparison, conversion, return semantics를 규정한다.

**[GCC]** C17 dialect, diagnostics, debug information과 `-Og` translation을 제공한다.

**[GDB]** debug info와 target state를 이용해 breakpoint·step·print·backtrace·watch를 수행한다.

**[Sanitizer]** supported checks가 instrumented path에서 report를 만들며 exact output은 tool version에 의존한다.

**[MIPS — 수업 기준]** simulator나 target debugger를 사용할 때 MIPS register·ABI 관찰로 별도 표시한다.

**[RISC-V — 병행 학습]** RISC-V toolchain·target state를 native x86-64 GDB 결과와 섞지 않는다.
## 8. 자주 하는 실수
- warning·GDB·ASan 하나를 correctness proof로 사용한다.
- `-g`와 optimization level을 같은 기능으로 설명한다.
- logic bug와 UB를 같은 범주로 부른다.
- source line과 instruction, frame과 C17 object를 일대일로 고정한다.
- sanitizer diagnostic을 C17 portable output으로 기록한다.
## 9. 필수 실습
예제에 function breakpoint를 걸고 index 변화를 관찰한 뒤 정상·not-found 입력을 재검증한다.
[28-12 exercise](../../exercises/28-debugging/28-12/README.md)
## 10. 추가 실습
- ★ 각 도구가 답하는 질문을 표로 만든다.
- ★★ expected diagnostic과 normal PASS를 분리한다.
- ★★★ 재현부터 regression까지 debugging report를 작성한다.
## 11. 확인 문제
1. `-Wall`은 모든 warning인가?
2. `-g`와 `-Og`의 차이는?
3. `step`과 `next`의 차이는?
4. backtrace가 C17 physical stack 보장인가?
5. ASan clean이 증명하지 못하는 것은?
6. logic bug와 UB는 어떻게 다른가?
7. debugging 완료를 어떤 evidence로 판단하는가?
## 12. 핵심 정리
- C17 rule과 GCC·GDB·sanitizer·OS 관찰을 분리한다.
- reproducible evidence와 한 가설씩의 검증으로 root cause를 찾는다.
- 수정 뒤 strict build, normal run, relevant tool scenario를 재검증한다.
## 13. 다음 Step
Part 29의 첫 Step은 [**29-1. 입력·출력·경계 조건**](../../C_CURRICULUM.md#part-29-c로-기본-알고리즘-구현)이다. 이번 Part에서는 Part 29 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 5.1.1.3. N1570은 **C11 공개 Committee Draft**이며 관련 diagnostic rule은 C17에서도 유지된다.
- [GCC manuals](https://gcc.gnu.org/onlinedocs/gcc/)
- [GDB User Manual](https://sourceware.org/gdb/current/onlinedocs/gdb.html/)
