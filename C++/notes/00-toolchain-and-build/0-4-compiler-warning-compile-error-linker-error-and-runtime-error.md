# Step 0-4 — compiler warning·compile error·linker error·runtime error

## 학습 목표

- warning, compile error, linker error, runtime problem을 발견 시점과 산출물로 구분한다.
- compile 성공과 전체 build 성공이 같은 뜻이 아님을 설명한다.
- 실제 GCC diagnostic을 읽고 어느 단계에서 멈췄는지 판단한다.
- build 성공과 program의 논리적 정확성을 구분한다.
- undefined behavior를 runtime error와 같은 뜻으로 사용하지 않는다.

## 선수 지식

- Step 0-1의 source file, executable, exit status
- Step 0-2의 compilation, object file, linking
- Step 0-3의 warning과 error, 기본 C++17 build option

## 1. Build 성공과 프로그램 정상은 다르다

source가 executable이 되려면 여러 단계를 통과하고, executable이 만들어진 뒤에는 별도의 실행 과정이 시작된다. 문제가 발견되는 위치를 알면 무엇을 먼저 고쳐야 하는지 좁힐 수 있다.

```text
source
  ↓
compiler  ── warning 또는 compile error
  ↓
object file
  ↓
linker    ── linker error
  ↓
executable
  ↓
execution ── runtime problem
```

이 그림은 이번 Step의 네 실험을 정리한 학습 모델이지 모든 도구와 실패를 빠짐없이 분류하는 절대 규칙은 아니다. 예를 들어 한 번의 `g++ source.cpp -o program` 명령이 compile과 link를 모두 수행할 수 있고, 실행 환경의 loader가 program 시작 전에 실패할 수도 있다.

또한 build 성공이나 warning 없음은 logic 정확성을 보장하지 않는다.

## 2. 문제를 어느 단계에서 발견하는가

| 종류 | 보통 발견 시점 | 이 실험의 최종 산출물 | 대표 상황 |
|---|---|---|---|
| warning | compile | executable 생성 | 사용하지 않는 local variable |
| compile error | compile | object/executable 미생성 | 선언되지 않은 이름 |
| linker error | link | object는 생성, executable 미생성 | 선언한 함수의 정의 누락 |
| runtime problem | execution | executable은 이미 생성 | 처리되지 않은 exception |

“보통”과 “이 실험”이라는 범위가 중요하다. warning을 error로 취급하도록 설정할 수 있고 이전 executable이 남아 있을 수도 있으므로, **실행한 command, 종료 상태, 새 산출물**을 함께 확인한다.

## 3. Warning

warning은 compiler가 의심스러운 상황을 알리는 diagnostic이며, 일반적으로 build를 계속할 수 있다.

```cpp
int main()
{
    int unused_value = 42;
    return 0;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic warning.cpp -o warning
```

GCC 13.3.0에서 다음 warning이 출력되었다.

```text
warning: unused variable ‘unused_value’ [-Wunused-variable]
```

command의 종료 상태는 `0`이었고 executable도 생성되어 실행할 수 있었다. 이 결과는 warning을 무시해도 된다는 뜻이 아니다. `unused_value`가 실수인지 의도인지 programmer가 확인해야 한다.

warning이 반드시 bug라는 뜻도 아니고, warning이 없다고 program이 정확하다는 뜻도 아니다. 이번 Step에서는 이 범주를 다른 실패와 비교하는 데 집중한다.

## 4. Compile Error

compile error는 compiler가 translation unit을 object code로 만들 수 없는 경우다. 다음 code는 선언되지 않은 이름을 사용한 semantic error다.

```cpp
int main()
{
    return missing_name;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic compile_error.cpp -o compile_error
```

현재 환경의 실제 diagnostic:

```text
compile_error.cpp:3:12: error: ‘missing_name’ was not declared in this scope
    3 |     return missing_name;
      |            ^~~~~~~~~~~~
```

GCC driver의 종료 상태는 `1`이었고, `compile_error` executable은 생성되지 않았다. `-c`로 compile-only를 시도해도 같은 위치에서 실패하여 object file이 생성되지 않았다.

괄호나 semicolon이 맞지 않는 syntax error도 이 단계에서 발견될 수 있다. 핵심은 **해당 translation unit을 object file로 만들지 못했다**는 점이다.

## 5. Linker Error

linker error는 각 translation unit의 compile이 성공한 뒤에도 발생할 수 있다. 다음 source에는 `add`의 declaration과 호출은 있지만 definition이 없다.

```cpp
int add(int a, int b);

int main()
{
    return add(1, 2);
}
```

먼저 linking 없이 object file까지만 만든다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -c linker_error.cpp -o linker_error.o
```

이 command는 diagnostic 없이 종료 상태 `0`으로 성공했고, `file linker_error.o`는 ELF relocatable object라고 표시했다. compiler는 호출에 필요한 declaration을 보았으므로 이 translation unit을 처리할 수 있었다.

이제 object file을 link한다.

```bash
g++ linker_error.o -o linker_error
```

현재 GCC/binutils 환경의 실제 결과:

```text
/usr/bin/ld: linker_error.o: in function `main':
linker_error.cpp:(.text+0x13): undefined reference to `add(int, int)'
collect2: error: ld returned 1 exit status
```

link command의 종료 상태는 `1`이었고 executable은 생성되지 않았다. linker가 program에 필요한 `add(int, int)` definition을 어느 object나 library에서도 찾지 못했기 때문이다.

definition을 가진 `add.cpp`를 추가해 object file로 만들고 함께 link하면 성공한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -c add.cpp -o add.o
g++ linker_error.o add.o -o linker_fixed
```

`g++ linker_error.cpp -o linker_error`처럼 한 명령을 사용해도 내부에는 compile과 link가 있다. 마지막 message에 `/usr/bin/ld`, `undefined reference`, `ld returned`가 나타난 이유는 `g++` driver가 호출한 linker 단계에서 실패했기 때문이다.

```text
compile 성공 ≠ 전체 program build 성공
```

## 6. Runtime Problem

`runtime error`는 C++ 표준의 모든 실행 실패를 하나로 묶는 엄밀한 분류명처럼 사용하지 않는다. 여기서는 **executable이 만들어지고 실행된 뒤 문제가 드러나는 실무적 범주**를 runtime problem이라고 부른다.

안전하게 반복 재현할 예제로 처리되지 않은 exception을 사용한다.

```cpp
#include <stdexcept>

int main()
{
    throw std::runtime_error("runtime example");
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic runtime_problem.cpp -o runtime_problem
./runtime_problem
echo $?
```

build는 diagnostic 없이 성공했고 executable이 생성되었다. Ubuntu GCC 13.3.0 환경에서 실행 결과는 다음과 같았다.

```text
terminate called after throwing an instance of 'std::runtime_error'
  what():  runtime example
Aborted (core dumped)
```

이 shell에서 관찰한 종료 상태는 `134`였다. C++ 규칙상 처리되지 않은 exception은 `std::terminate` 호출로 이어지고, 기본 terminate handler는 `std::abort`를 호출한다. 그러나 diagnostic 문구, `core dumped` 표시, 숫자 종료 상태는 환경 차이가 있으므로 C++ 표준이 같은 text와 number를 보장한다고 설명해서는 안 된다.

runtime problem에는 비정상 종료뿐 아니라 잘못된 출력, 응답 없음, resource 실패처럼 process가 즉시 종료되지 않는 문제도 포함될 수 있다.

## 7. Exit Status도 단계와 함께 읽기

종료 상태 `0`은 일반적으로 command 성공을 뜻하고 non-zero는 실패 또는 특정 상태를 보고한다. 하지만 program이 의도적으로 non-zero를 반환할 수도 있다. 실제로 수정한 linker 예제는 link에 성공했지만 `main`이 `add(1, 2)`의 값 `3`을 반환하므로 실행 종료 상태가 `3`이었다.

따라서 build command의 exit status와 program의 exit status를 구분한다. 앞의 비교표와 함께 읽으면 어느 단계까지 성공했는지 판단할 수 있다.

## 8. Bug와 Diagnostic의 차이

bug는 program의 결함을 넓게 가리키는 말이다. compile error, linker error, runtime problem은 문제가 발견된 단계나 도구를 설명하지만 모든 bug를 포함하지는 않는다.

```cpp
#include <iostream>

int add(int a, int b)
{
    return a - b;
}

int main()
{
    std::cout << "2 + 3 = " << add(2, 3) << '\n';
    return 0;
}
```

이 program은 기본 option으로 warning 없이 compile·link되고 종료 상태 `0`으로 실행되었다. 실제 출력은 다음과 같다.

```text
2 + 3 = -1
```

문법과 type은 유효하지만 덧셈이라는 의도와 구현이 다르므로 logic bug다. compiler는 함수 이름 `add`만 보고 programmer의 의도를 증명할 수 없다.

## 9. Undefined Behavior와 Runtime Error

다음 out-of-bounds access는 undefined behavior를 일으킨다.

```cpp
int main()
{
    int values[3]{};
    values[10] = 5;
    return values[0];
}
```

GCC 13.3.0에서는 기본 option으로 compile과 link가 성공해 executable이 생성되었다. 그러나 undefined behavior는 C++ 표준이 그 실행의 동작을 요구하지 않는 상황이다. 반드시 crash하거나 특정 runtime diagnostic을 출력한다고 말할 수 없다.

이 예제의 실행 결과를 정답처럼 관찰하지 않는 이유도 여기에 있다. 한 환경에서 조용히 끝나거나 crash한 결과 모두 다른 실행을 예측하는 근거가 되지 않는다. 이후 sanitizer Step에서는 구현이 제공하는 추가 진단 도구를 사용하지만, sanitizer 결과도 C++ 표준의 동작 보장과는 구분한다.

```text
undefined behavior ≠ runtime error라는 고정된 결과
```

## 10. 자주 하는 실수

- **모든 `g++` 실패를 compile error라고 부른다:** driver가 호출한 linker에서 실패할 수도 있다.
- **object file 생성을 전체 build 성공으로 본다:** 최종 executable을 만들려면 link가 남아 있다.
- **executable이 있으면 이번 build가 성공했다고 생각한다:** 이전 build 파일일 수 있으므로 command 상태와 생성 시점을 확인한다.
- **runtime problem은 항상 OS가 강제 종료한다고 생각한다:** 잘못된 결과나 정지처럼 process가 계속되는 문제도 있다.
- **logic bug도 compiler가 알아야 한다고 생각한다:** programmer의 의도가 language rule에 포함되지 않을 수 있다.
- **undefined behavior를 예측 가능한 runtime error로 설명한다:** 표준이 결과를 요구하지 않는다.

## 11. 핵심 정리

- warning은 의심 지점을 알리지만 일반적으로 build를 계속할 수 있다.
- compile error는 translation unit을 object file로 만드는 과정에서 실패한 경우다.
- linker error는 필요한 definition을 연결하지 못해 executable을 만들지 못한 경우다.
- runtime problem은 executable 실행 중 문제가 드러나는 실무적 범주다.
- `g++` 한 명령 안에서도 compile과 link는 서로 다른 단계다.
- build 성공, exit status `0`, warning 없음은 각각 logic 정확성의 증명이 아니다.
- undefined behavior에는 표준이 요구하는 고정된 실행 결과가 없다.

## 12. 확인 문제

1. warning이 있어도 executable이 생성될 수 있는 이유는 무엇인가?
2. compile-only가 성공하고 link가 실패할 수 있는 이유는 무엇인가?
3. `undefined reference`가 linker 단계 문제임을 어떤 관찰로 판단할 수 있는가?
4. executable이 존재해도 이번 build의 성공을 보장하지 않는 이유는 무엇인가?
5. 처리되지 않은 exception에서 표준이 보장하는 부분과 이 환경에서만 관찰한 부분을 구분하라.
6. logic bug가 warning 없이 build될 수 있는 이유는 무엇인가?
7. undefined behavior와 runtime error를 같은 의미로 쓰면 왜 잘못인가?
8. 한 번의 `g++` 명령에서 compile error와 linker error가 모두 가능할 수 있는 이유는 무엇인가?

## 참고 자료

- [GCC 13.3 — Overall Options](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc/Overall-Options.html)
- [GCC 13.3 — Link Options](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc/Link-Options.html)
- [cppreference — Exception handling](https://en.cppreference.com/w/cpp/language/exceptions.html)
- [cppreference — std::terminate](https://en.cppreference.com/w/cpp/error/terminate.html)
- [cppreference — Undefined behavior](https://en.cppreference.com/w/cpp/language/ub.html)

## 다음 Step

다음은 **Step 0-5 — GDB: breakpoint·step·backtrace·variable inspection**이다.

현재 Step의 실습은 [Step 0-4 Exercise](../../exercises/00-toolchain-and-build/0-4/README.md)에서 수행한다.
