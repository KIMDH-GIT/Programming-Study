# 28-8. 변수·메모리 출력
## 1. 학습 목표
- `print`로 typed expression 값을 읽는다.
- `x`로 지정 address의 memory를 형식에 맞춰 관찰한다.
- raw bytes, address, C object value를 구분한다.
## 2. 선수 지식
28-6의 breakpoint와 Part 14~15의 pointer·array를 안다.
## 3. 핵심 개념
**[GDB]** `print expression`은 현재 execution context에서 source language expression을 평가하고 값을 표시한다. `x/nfu address`는 memory를 count·format·unit 지정에 따라 조사한다.

초보 실습에서는 read-only expression을 사용한다. GDB expression으로 값을 대입하거나 function을 호출하면 debuggee state가 바뀔 수 있으므로 필수 범위에서 제외한다.
## 4. 문법
```gdb
break main
run
next 2
print values[0]
print total
x/3dw values
info locals
```

`break main`은 function body 진입 직전에 멈출 수 있으므로 이 예제에서는 `next 2`로 두 declaration의 initialization을 끝낸 뒤 값을 읽는다. `x/3dw`는 세 개의 word를 signed decimal 형식으로 관찰하는 GDB syntax다. target의 object representation과 type·alignment를 고려해야 하며 raw memory를 곧바로 모든 C value와 동일시하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[] = {4, 5, 6};
    int total = values[0] + values[1] + values[2];

    printf("%d\n", total);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o gdb_data
gdb ./gdb_data
```
## 6. 코드 해석
initialization 뒤 `print values[1]`은 5, `print total`은 15를 관찰할 수 있다. 정상 실행 output은 `15`다.
## 7. 내부 동작
**[C17]** 배열 object, element type, lifetime을 규정한다.

**[GDB]** debug type information으로 `print`를 해석하고 target memory에서 bytes를 읽는다.

**[OS / ABI / CPU]** address representation, byte order, register placement는 target 관찰이다. native x86-64 결과를 MIPS나 RISC-V register 관찰로 부르지 않는다.

**[MIPS — 수업 기준]** MIPS debugger의 register·memory view는 해당 target 기준으로 별도 해석한다.

**[RISC-V — 병행 학습]** RISC-V target에서도 ABI와 ISA 정보를 별도로 확인한다.
## 8. 자주 하는 실수
- `print`가 source에 assignment를 추가한다고 생각한다.
- memory dump bytes와 typed C value를 같은 개념으로 본다.
- invalid pointer를 GDB로 읽으면 C access가 valid해진다고 생각한다.
- host register 이름을 MIPS/RISC-V에 그대로 적용한다.
## 9. 필수 실습
배열 initialization 뒤 세 element, total, 연속 memory를 read-only로 관찰한다.
[28-8 exercise](../../exercises/28-debugging/28-8/README.md)
## 10. 추가 실습
- ★ `print`와 `x` 결과를 비교한다.
- ★★ pointer와 pointee를 각각 출력한다.
- ★★★ optimization level별 local 관찰을 비교한다.
## 11. 확인 문제
1. `print`는 무엇을 평가하는가?
2. `x` 명령의 목적은?
3. raw bytes와 C value가 다른 이유는?
4. debugger expression side effect를 피하는 이유는?
5. current target 확인이 필요한 이유는?
## 12. 핵심 정리
- typed state는 `print`, raw memory는 `x`로 목적을 나눠 관찰한다.
- 대상의 lifetime·bounds·type 규칙은 여전히 C17을 따른다.
- debugger state 변경보다 read-only evidence를 우선한다.
## 13. 다음 Step
[28-9. `backtrace`](28-9-backtrace.md)
## 14. 참고 자료
- [GDB: Examining Data](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Data.html)
- [GDB: Examining Memory](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Memory.html)
