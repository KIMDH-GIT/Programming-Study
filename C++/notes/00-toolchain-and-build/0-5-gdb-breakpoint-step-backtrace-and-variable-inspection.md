# Step 0-5 — GDB: breakpoint·step·backtrace·variable inspection

## 학습 목표

- debugger가 필요한 이유와 `-g`, GDB의 역할을 구분한다.
- breakpoint에서 멈춘 source line을 확인하고 실행을 제어한다.
- `run`·`continue`, `next`·`step`을 상황에 맞게 선택한다.
- 현재 stack frame의 variable과 call stack을 관찰한다.
- 관찰한 실행 상태로 logic bug의 범위를 좁힌다.

## 선수 지식

- Step 0-2의 compile·link와 executable
- Step 0-3의 `-g`와 debug information
- Step 0-4의 build 성공과 logic 정확성의 차이

## 1. Debugger가 필요한 이유

warning 없이 build되고 정상 종료한 program도 잘못된 값을 출력할 수 있다. source만 읽어서 원인을 바로 찾기 어려울 때는 실행 중인 실제 상태를 관찰해야 한다.

```text
실행을 특정 위치에서 멈춘다.
→ 현재 source line과 variable을 확인한다.
→ 한 line씩 진행하며 호출 관계를 본다.
→ 예상과 실제가 처음 달라지는 범위를 좁힌다.
```

GDB 같은 debugger는 이 관찰을 돕지만 bug를 자동으로 검사하거나 수정하지 않는다.

## 2. Debug Information과 GDB

Step 0-3의 `-g`는 debugger가 사용할 source-level debug information을 생성하도록 compiler에 요청한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

```text
-g
    source line·variable·type 등에 관한 debug information 생성

GDB
    executable 실행 제어와 현재 상태 관찰
```

`-g`는 “debug mode”, GDB 실행, bug 검사라는 뜻이 아니다. `-g` 없이도 program은 실행할 수 있지만 source line과 local variable을 연결하는 정보는 부족할 수 있다.

GCC는 `-g`와 optimization option을 함께 사용할 수 있다. 다만 optimized program에서는 variable이나 source-level stepping이 직관적이지 않을 수 있다. 이번 Step은 별도 optimization option 없이 실행 제어를 익힌다.

## 3. 예제 프로그램 준비

다음 program은 `main → calculate → multiply`의 세 단계 호출 구조를 가진다.

```cpp
#include <iostream>

int multiply(int a, int b)
{
    int result = a * b;
    return result;
}

int calculate(int value)
{
    int doubled = multiply(value, 2);
    return doubled + 1;
}

int main()
{
    int input = 5;
    int answer = calculate(input);

    std::cout << answer << '\n';
    return 0;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
./main
```

일반 실행 결과는 `11`이다. 이제 output만 보는 대신 계산 과정을 GDB로 관찰한다.

## 4. GDB 시작과 현재 위치

```bash
gdb ./main
```

`(gdb)` prompt에서는 shell이 아니라 GDB가 command를 기다린다. GDB가 제어하는 program을 debuggee라고 부르기도 하며, GDB process와 debuggee는 같은 대상이 아니다.

`list`는 source 일부를 보여 준다. program이 멈추면 GDB가 표시한 file, line number, source line을 읽어 현재 위치를 확인한다.

```text
(gdb) list main
```

## 5. Breakpoint와 `run`

breakpoint는 program execution을 특정 위치에서 일시 정지시키기 위한 debugger의 설정이다.

```text
(gdb) break main
Breakpoint 1 at 0x11bb: file main.cpp, line 17.
(gdb) run
Breakpoint 1, main () at main.cpp:17
17        int input = 5;
```

위 line과 address는 Ubuntu/GDB 15.1 검증 결과다. 환경과 file path에 따라 세부 text는 달라질 수 있다.

`run`은 program을 시작하고 breakpoint까지 실행한다. 표시된 line은 이제 실행할 현재 위치이므로 statement가 이미 완료되었다고 가정하지 않는다. breakpoint는 source를 수정하는 기능이 아니라 GDB가 debuggee 실행을 제어하는 설정이다.

## 6. `next`와 `step`

두 명령은 현재 source line을 실행하지만 함수 호출에서 선택이 달라진다.

```text
next
    호출이 있어도 보통 내부로 들어가지 않고
    현재 stack frame의 다음 source line에서 멈춘다.

step
    source 정보를 사용할 수 있으면
    현재 line에서 호출하는 함수 내부로 진입한다.
```

앞 절의 GDB를 종료한 뒤 새 GDB session을 시작하고, 동일한 호출 line에서 차이를 확인하려고 `calculate`에 breakpoint를 설정했다.

```text
(gdb) break calculate
(gdb) run
Breakpoint 1, calculate (value=5) at main.cpp:11
11        int doubled = multiply(value, 2);
```

`next`를 사용한 실제 결과:

```text
(gdb) next
12        return doubled + 1;
(gdb) print doubled
$1 = 10
```

`multiply`는 실행되었지만 내부 source line에서 멈추지 않았고 현재 frame은 `calculate`다.

같은 breakpoint에서 다시 시작한 별도 실행에 `step`을 사용했다.

```text
(gdb) step
multiply (a=5, b=2) at main.cpp:5
5         int result = a * b;
```

callee 계산을 조사할 필요가 있으면 `step`, 호출 결과만 필요하면 `next`가 출발점이다. 실제 정지 위치는 debug information, source mapping, library, optimization의 영향을 받으므로 두 명령을 절대적인 한 줄 이동 규칙으로 설명하지 않는다.

## 7. `run`과 `continue`

```text
run
    program 시작 또는 restart

continue
    현재 stopped execution 재개
    다음 breakpoint, signal, program 종료 등까지 진행
```

`calculate`와 `multiply`에 breakpoint를 둔 검증에서 `run`은 먼저 `calculate`에서 멈췄고, `continue`는 그 상태에서 실행을 재개해 `multiply`에서 다시 멈췄다.

```text
(gdb) continue
Continuing.

Breakpoint 2, multiply (a=5, b=2) at main.cpp:5
```

`continue`는 다음 source line만 실행하는 `next`와 다르다.

## 8. Variable Inspection

variable 값은 “언제, 어느 stack frame에서 보았는가”와 함께 해석한다. `multiply`의 계산 line을 실행한 뒤 관찰한 결과다.

```text
(gdb) print a
$1 = 5
(gdb) print b
$2 = 2
(gdb) next
6         return result;
(gdb) print result
$3 = 10
(gdb) info locals
result = 10
```

`print`는 선택된 frame의 문맥에서 expression을 평가한다. `info locals`는 현재 선택된 frame의 local variable을 보여 주며 parameter까지 모두 보여 주는 명령이라는 뜻은 아니다.

`multiply` frame에서 `main`의 local 이름을 요청하면 scope가 다르다.

```text
(gdb) print input
No symbol "input" in current context.
```

초기화 statement가 아직 실행되지 않았다면 표시된 저장 공간의 값을 의도한 variable 값으로 해석하면 안 된다. 현재 line과 실행 시점을 먼저 확인한다.

## 9. Stack Frame과 Call Stack

각 function 호출에는 parameter, local variable, 돌아갈 위치 같은 실행 문맥이 필요하다. 이 개별 호출의 문맥을 stack frame이라고 생각할 수 있다. 현재 함수와 아직 끝나지 않은 caller frame들이 이어진 관계가 call stack이다.

```text
main()
  ↓ calls
calculate()
  ↓ calls
multiply()
```

이번 Step에서는 이 호출 관계까지만 다루며 stack memory layout, register, ABI로 확장하지 않는다.

## 10. `backtrace`

`backtrace`, 단축형 `bt`는 현재 정지 시점의 call stack을 frame별로 요약한다.

```text
(gdb) backtrace
#0  multiply (a=5, b=2) at main.cpp:6
#1  calculate (value=5) at main.cpp:11
#2  main () at main.cpp:18
```

`#0`은 현재 frame이고 큰 번호는 caller 방향이다. address나 세부 text는 환경에 따라 달라질 수 있다.

`backtrace`는 과거에 실행한 모든 함수의 기록이 아니다. 이미 return하여 사라진 호출까지 보관하는 history가 아니라 **현재 정지 시점에 활성화된 frame들**을 보여 준다.

## 11. Debug Information이 없을 때

같은 source를 `-g` 유무만 다르게 build한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic main.cpp -o main-no-g
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main-g
```

두 executable 모두 `11`을 출력했다. 현재 환경의 `main-no-g`에는 함수 이름 일부가 남아 `break calculate`가 가능했지만 source와 local 정보는 제한되었다.

```text
Breakpoint 1, calculate(int) ()
(gdb) list
No symbol table is loaded.  Use the "file" command.
(gdb) info locals
No symbol table info available.
(gdb) print value
No symbol table is loaded.  Use the "file" command.
```

`-g`가 없으면 GDB를 전혀 사용할 수 없다고 일반화하지 않는다. 이 executable에서는 함수 단위 breakpoint와 제한된 backtrace가 가능했지만 source line과 local variable의 연결 정보가 부족했다.

## 12. 작은 Logic Bug 좁히기

exercise 후반에는 정상 build되고 종료 상태도 `0`이지만 예상과 다른 값을 출력하는 variant를 다음 순서로 조사한다.

```text
1. 예상 output과 실제 output을 비교한다.
2. 함수 경계에 breakpoint를 두고 run한다.
3. backtrace로 호출 경로를 확인한다.
4. step으로 callee에 들어간다.
5. next와 print로 중간 값을 확인한다.
6. caller의 최종 값과 비교한다.
7. 예상과 실제가 처음 갈라지는 source 범위를 찾는다.
```

처음부터 정답으로 의심한 line에 멈추는 것이 아니라, function에 전달된 값과 반환되는 값을 순서대로 비교해 조사 범위를 좁힌다. 중간 값 하나가 맞다고 전체 함수가 맞다고 결론 내리지 않고, 잘못된 최종 값만 보고 모든 callee를 의심하지도 않는다. signal에서 멈추는 기능과 crash 분석은 이번 Step에서 확장하지 않는다.

## 13. 자주 하는 실수

- **`-g`를 GDB 실행 option이라고 생각한다:** compiler가 debug information을 생성하는 option이다.
- **멈춘 line이 이미 실행됐다고 가정한다:** 현재 statement의 실행 전후를 확인한다.
- **`run`과 `continue`를 섞는다:** 시작·restart와 stopped execution 재개를 구분한다.
- **`next`와 `step`을 같은 이동으로 본다:** 함수 호출 내부 진입 여부가 핵심이다.
- **variable 값을 시점 없이 기록한다:** 현재 line, scope, frame을 함께 확인한다.
- **`info locals`가 모든 frame을 보여 준다고 생각한다:** 현재 선택된 frame 기준이다.
- **`backtrace`를 전체 실행 history라고 설명한다:** 현재 활성 call stack의 요약이다.
- **debugger가 보여 준 값을 원인이라고 바로 단정한다:** 예상과 비교해 범위를 좁힌다.

## 14. 핵심 정리

- breakpoint를 설정한 뒤 `run`으로 시작하고 `continue`로 멈춘 execution을 재개한다.
- `next`는 보통 호출을 지나가고, `step`은 가능한 경우 callee 내부로 들어간다.
- `print`와 `info locals`는 현재 frame과 현재 정지 시점의 문맥에서 해석한다.
- stack frame은 개별 호출의 문맥이고 call stack은 활성 frame의 호출 관계다.
- `backtrace`는 현재 call stack이지 전체 실행 기록이 아니다.
- `-g`가 없어도 program은 실행되지만 source-level 관찰은 제한될 수 있다.

## 15. 확인 문제

1. breakpoint에서 표시된 source line은 이미 실행된 statement인가, 이제 실행할 statement인가?
2. `calculate`의 호출 결과만 확인할 때와 내부 계산을 조사할 때 각각 어떤 stepping command를 선택하는가?
3. `multiply` frame에서 `input`을 바로 출력할 수 없는 이유는 무엇인가?
4. `multiply`가 return한 뒤 `backtrace`에 해당 frame이 남아 있지 않은 이유는 무엇인가?
5. no-`-g` executable에서도 가능한 관찰과 제한되는 관찰을 구분하라.
6. 예상 output과 실제 output이 다를 때 breakpoint 이후 어떤 순서로 조사 범위를 좁힐 것인가?
7. breakpoint에서 멈춘 뒤 `run`과 `continue`를 각각 입력하면 실행 흐름이 어떻게 달라지는가?
8. `next`로 `multiply` 호출을 지난 뒤 현재 frame과 확인할 수 있는 local variable은 무엇인가?
9. breakpoint를 추가하거나 제거해도 source code 자체가 바뀌지 않는 이유는 무엇인가?
10. `-g` build와 GDB 실행은 debugging 과정에서 어떤 순서와 역할로 연결되는가?

## 참고 자료

- [GNU GDB — Setting Breakpoints](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Set-Breaks.html)
- [GNU GDB — Starting Your Program](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Starting.html)
- [GNU GDB — Continuing and Stepping](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Continuing-and-Stepping.html)
- [GNU GDB — Examining Data](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Data.html)
- [GNU GDB — Backtraces](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Backtrace.html)
- [GCC — Options for Debugging Your Program](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html)

## 다음 Step

다음은 **Step 0-6 — AddressSanitizer와 UndefinedBehaviorSanitizer**이다.

현재 Step의 실습은 [Step 0-5 Exercise](../../exercises/00-toolchain-and-build/0-5/README.md)에서 수행한다.
