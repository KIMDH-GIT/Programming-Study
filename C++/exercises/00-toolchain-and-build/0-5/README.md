# Step 0-5 Exercise — GDB

학습 문서: [Step 0-5 — GDB: breakpoint·step·backtrace·variable inspection](../../../notes/00-toolchain-and-build/0-5-gdb-breakpoint-step-backtrace-and-variable-inspection.md)

## 실습 목적

- debug information을 포함한 executable을 GDB로 실행한다.
- breakpoint, source line, variable, call stack을 직접 관찰한다.
- 같은 호출 line에서 `next`와 `step`을 비교한다.
- 실제 값으로 logic bug의 범위를 좁힌다.

## 준비

```bash
cd exercises/00-toolchain-and-build/0-5
g++ --version
gdb --version
```

- GCC version:
- GDB version:

`main.cpp`를 작성한다.

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

현재 function, source line, variable 값을 command마다 기록한다. GDB는 `quit`, 단축형 `q`로 끝낸다.

## 실습 1 — Debug Build

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main-g
g++ -std=c++17 -Wall -Wextra -pedantic main.cpp -o main-no-g
./main-g
./main-no-g
```

- 두 build의 diagnostic / 일반 실행 결과:
- `-g`가 일반 실행의 필수 조건인지 설명:

## 실습 2 — Breakpoint와 `run`

```bash
gdb ./main-g
```

```gdb
break main
run
list
next
print input
continue
quit
```

- breakpoint file·line / `run` 후 function·line:
- `next` 전후 `input` 초기화 상태:
- breakpoint가 source를 수정하지 않는 근거:

## 실습 3 — `next`와 `step` 비교

첫 GDB 실행:

```bash
gdb ./main-g
```

```gdb
break calculate
run
next
print doubled
quit
```

두 번째 GDB 실행:

```bash
gdb ./main-g
```

```gdb
break calculate
run
step
print a
print b
next
print result
quit
```

| command | 호출 뒤 멈춘 function | source line의 역할 |
|---|---|---|
| `next` |  |  |
| `step` |  |  |

- callee 내부 조사 / 호출 결과 확인에 각각 선택할 command와 이유:

## 실습 4 — Variable Inspection

```bash
gdb ./main-g
```

```gdb
break multiply
run
next
print result
info locals
print input
backtrace
quit
```

- `result` 값과 확인 시점:
- `info locals` 결과:
- `print input` 결과와 scope 관점의 이유:
- 값을 기록할 때 function·line·frame이 필요한 이유:

## 실습 5 — `continue`와 Backtrace

```bash
gdb ./main-g
```

```gdb
break calculate
break multiply
run
continue
backtrace
continue
quit
```

- `run`과 첫 `continue`가 각각 멈춘 function:
- `#0`, `#1`, `#2`의 function 이름:
- `backtrace`가 과거 실행 전체 기록이 아닌 이유:

debug information이 없는 executable도 비교한다.

```bash
gdb ./main-no-g
```

```gdb
break calculate
run
list
info locals
print value
backtrace
continue
quit
```

- 가능한 관찰 / 제한된 관찰:
- `-g`가 없으면 GDB를 전혀 사용할 수 없다고 단정하면 안 되는 이유:

## 실습 6 — Logic Bug 추적

다음 `bug.cpp`의 의도한 결과는 `11`이지만 logic bug가 하나 있다. 정답을 먼저 확정하지 말고 GDB 관찰로 범위를 좁힌다.

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
    int answer = calculate(input - 1);

    std::cout << answer << '\n';
    return 0;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g bug.cpp -o bug
./bug
gdb ./bug
```

작성한 file에서 `main`과 output statement의 line을 확인한 뒤 breakpoint를 설정한다. 위 code를 그대로 입력하면 output statement는 line 20이며, line이 다르면 아래 `20`을 실제 확인한 번호로 바꾼다. 다음 흐름은 function에 전달되는 값부터 callee의 계산 결과까지 순서대로 확인한다.

```gdb
break main
break calculate
break multiply
break bug.cpp:20
run
next
print input
step
print value
backtrace
step
print a
print b
next
print result
continue
print answer
quit
```

- 예상 output / 실제 output:
- `input` / `value` / `a` / `b` / `result` / `answer`:
- 예상과 실제가 처음 갈라지는 source 범위:
- 최소 수정과 수정 후 실행 결과:

## 관찰 결과 정리

| 대상 | command | frame·line | 관찰 | 해석 |
|---|---|---|---|---|
| main 시작 |  |  |  |  |
| calculate 호출 |  |  |  |  |
| multiply 내부 |  |  |  |  |
| call stack |  |  |  |  |
| no-g executable |  |  |  |  |
| logic bug |  |  |  |  |

## 생각해 볼 문제

1. `-g`와 GDB는 각각 어떤 역할인가?
2. breakpoint는 source code를 수정하는가?
3. `run`과 `continue`의 차이는 무엇인가?
4. `next`와 `step`의 차이는 무엇인가?
5. `print`가 보여 주는 variable 값은 어느 시점의 값인가?
6. `info locals`는 무엇을 보여 주는가?
7. `backtrace`는 program의 전체 실행 기록인가?
8. call stack과 stack frame은 어떤 관계인가?
9. `-g` 없이도 executable을 실행할 수 있는가?
10. build가 정상이어도 debugger가 필요한 이유는 무엇인가?

## 힌트

- 표시된 source line이 아직 실행 전일 수 있다.
- `next`와 `step`은 같은 호출 line에서 비교한다.
- 이름을 찾지 못하면 현재 frame과 scope를 확인한다.
- `#0`은 현재 frame이고 큰 번호는 caller 방향이다.
- no-g 비교에서는 무엇이 제한되는지 기록한다.
- bug 추적에서는 값이 맞는 마지막 지점과 틀린 첫 지점을 찾는다.

## 완료 체크

- [ ] GCC와 GDB version을 기록했다.
- [ ] `-g` 유무 build를 모두 실행했다.
- [ ] breakpoint와 `run`으로 source line에서 멈췄다.
- [ ] 동일 호출 line에서 `next`와 `step`을 비교했다.
- [ ] `print`와 `info locals`로 현재 frame을 관찰했다.
- [ ] `run`과 `continue`의 차이를 실행으로 확인했다.
- [ ] `backtrace`에서 `multiply → calculate → main`을 읽었다.
- [ ] no-g executable의 source·local 정보 제한을 관찰했다.
- [ ] logic bug의 중간 값과 최종 값을 추적했다.
- [ ] 수정 후 warning 없는 build와 의도한 output을 확인했다.
