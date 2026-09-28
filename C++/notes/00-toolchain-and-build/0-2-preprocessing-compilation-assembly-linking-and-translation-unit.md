# Step 0-2 — 전처리·컴파일·어셈블·링크와 translation unit

## 학습 목표

- 하나의 build를 preprocessing, compilation proper, assembly, linking으로 구분한다.
- 각 단계의 입력, 처리, 출력을 설명한다.
- source file과 translation unit이 같은 개념이 아님을 설명한다.
- `g++ -E`, `-S`, `-c`로 GCC의 중간 산출물을 관찰한다.
- object file과 executable의 역할을 구분한다.
- C++ 표준의 translation 개념과 GCC/Linux 구현을 구분한다.

## 선수 지식

- Step 0-1의 source file, executable, build 개념
- `g++`로 C++17 프로그램을 build하고 실행하는 방법
- `#include <iostream>`과 `std::cout`을 사용한 최소 프로그램

## 1. 하나의 `g++` 명령 뒤에서 일어나는 일

Step 0-1에서는 `g++ -std=c++17 hello.cpp -o hello`를 하나의 build 명령으로 사용했다. GCC 문서에서는 이 과정을 preprocessing → compilation proper → assembly → linking으로 구분한다.

`g++`는 GNU C++ compiler driver다. 모든 처리를 혼자 수행하는 단일 변환기라기보다, 입력과 option에 맞춰 C++ front end, assembler, linker 같은 관련 도구를 호출하고 전체 과정을 조정한다.

| 단계 | 주된 입력 | 처리 | 주된 출력 |
|---|---|---|---|
| preprocessing | C++ source와 포함된 header | directive 처리와 macro 확장 | preprocessed source |
| compilation proper | translation unit | 문법·의미 분석과 code generation | assembly code |
| assembly | assembly code | target용 명령과 data를 object 형식으로 변환 | object file |
| linking | object file과 필요한 library | 필요한 참조를 연결해 program image 구성 | executable |

실제 구현은 일부 단계를 내부에서 결합할 수 있다. 이 표는 학습과 관찰을 위한 논리적 구분이다.

## 2. Preprocessing

preprocessing은 `#`으로 시작하는 preprocessing directive를 처리한다.

- `#include <iostream>`은 지정한 header의 내용을 preprocessing 과정에 포함시킨다.
- `#define MESSAGE "Hello, build stages!"` 뒤의 `MESSAGE`는 macro 규칙에 따라 문자열로 대체된다.
- `#if`, `#ifdef`, `#ifndef` 같은 directive는 조건에 따라 일부 source를 포함하거나 제외한다.

정확한 include 규칙, macro 종류와 안전한 사용법은 이번 Step의 범위가 아니다. 현재는 이 결과가 translation unit 형성에 참여한다는 관계가 중요하다.

GCC에서 preprocessing까지만 수행하려면 `-E`를 사용한다.

```bash
g++ -std=c++17 -E hello.cpp -o hello.i
```

`-E`는 compiler proper를 실행하기 전에 멈춘다. `hello.i`에는 header에서 포함된 내용과 macro가 처리된 결과가 들어가므로 원래 `hello.cpp`보다 훨씬 길 수 있다.

이 교재는 관찰할 파일 이름으로 `hello.i`를 사용한다. GCC가 preprocessed C++ 입력을 확장자로 판별할 때 사용하는 관례는 `.ii`이므로, 파일 이름 자체를 C++ 표준 개념과 혼동하지 않는다.

## 3. Source File과 Translation Unit

translation unit을 “`.cpp` 파일 하나”라고 정의하면 정확하지 않다.

```text
source file
    ↓ preprocessing
translation unit
```

source file은 저장 장치에 있는 입력 text다. translation unit은 preprocessing directive가 처리되고, 포함된 header 내용과 macro 확장 결과가 반영된 뒤 C++ translation의 다음 단계에서 다루는 개념적 단위다. 따라서 일반적으로 **source file ≠ translation unit**이다.

하나의 `hello.cpp`가 `<iostream>`을 include하면 translation unit에는 `hello.cpp`에 직접 적지 않은 많은 선언도 들어온다.

Header file은 보통 `#include`를 통해 source file의 translation unit을 형성하는 데 참여한다. Header가 항상 독립적인 translation unit으로 compile된다고 설명해서는 안 된다.

C++ 표준의 translation phase는 구현이 따라야 할 의미를 “그 순서대로 일어난 것처럼” 기술한다. GCC가 실제로 `.i`, `.s`, `.o` 파일을 반드시 저장해야 한다는 뜻은 아니다.

## 4. Compilation

preprocessing 뒤에는 C++ token의 문법과 의미를 분석하는 compilation proper가 이어진다.

이 과정에서 구현은 예를 들어 다음을 확인한다.

- 문장이 C++ 문법에 맞는가
- 사용한 이름과 type 관계가 유효한가
- 표현식과 함수 호출의 의미가 올바른가
- 이후 단계에 필요한 target code를 어떻게 만들 것인가

GCC의 `-S` option은 compilation proper 뒤에서 멈추고 assembly code를 남긴다. Parser 내부 구현, AST 세부 구조, optimizer 알고리즘, backend 구조는 이번 Step에서 다루지 않는다.

## 5. Assembly

다음 명령은 preprocessing과 compilation proper를 수행한 뒤 assembly code를 생성한다.

```bash
g++ -std=c++17 -S hello.cpp -o hello.s
```

`hello.s`는 사람이 text로 열어 볼 수 있는 assembly source다. 내용은 CPU architecture, 운영체제, compiler version과 option에 따라 달라질 수 있다.

이번 Step의 목적은 assembly instruction을 해석하는 것이 아니다. compilation proper의 출력이 assembler의 입력이 될 수 있다는 단계 관계를 확인하는 것이다.

## 6. Object File

assembler는 assembly code를 object file로 변환한다. 다음 명령은 source 처리와 assembly까지 수행하지만 linking은 하지 않는다.

```bash
g++ -std=c++17 -c hello.cpp -o hello.o
```

```text
hello.cpp
    ↓ preprocessing
translation unit
    ↓ compilation proper
assembly code
    ↓ assembly
hello.o
```

`hello.o`에는 target machine code와 linking에 필요한 정보가 들어갈 수 있다. 그러나 아직 필요한 모든 참조가 연결된 최종 executable은 아니다.

Linux 환경에서는 다음으로 형식을 관찰할 수 있다.

```bash
file hello.o
```

출력의 정확한 architecture와 형식은 환경에 따라 다르다. C++ 표준은 `.o` 확장자나 ELF object file을 요구하지 않는다.

## 7. Linking

linking은 object file과 program에 필요한 library 구성 요소를 모아 실행 가능한 program image를 만든다.

```text
object file(s)
        +
필요한 library와 참조 대상
        ↓
linker
        ↓
executable
```

앞에서 만든 object file은 다음처럼 link한다.

```bash
g++ hello.o -o hello
```

여기서도 `g++` driver를 사용하면 C++ 프로그램 linking에 필요한 설정과 standard library 연결을 driver가 처리한다.

source file이 하나뿐이어도 `std::cout` 같은 외부 구성 요소와 실행 환경에 필요한 부분을 연결해야 하므로 linking 단계가 필요하다.

복잡한 symbol resolution, static/shared library, ABI, ODR은 이후 Step의 주제다. 현재 핵심은 compilation과 linking이 서로 다른 작업이라는 사실이다.

## 8. 전체 Build 흐름

```text
C++ source file: hello.cpp
        ↓ preprocessing
translation unit
        ↓ compilation proper
assembly code: hello.s
        ↓ assembly
object file: hello.o
        ↓ linking
executable: hello
```

GCC에서 관찰하는 `hello.i`, `hello.s`, `hello.o`, `hello`는 각 단계 이해를 돕는 외부 파일이다. C++ 표준의 모든 translation phase와 이 파일들이 일대일로 대응한다고 해석하지 않는다.

## 9. GCC에서 각 단계 직접 관찰

다음 `hello.cpp`를 모든 명령에서 동일하게 사용한다.

```cpp
#include <iostream>

#define MESSAGE "Hello, build stages!"

int main()
{
    std::cout << MESSAGE << '\n';
    return 0;
}
```

```bash
g++ -std=c++17 -E hello.cpp -o hello.i
g++ -std=c++17 -S hello.cpp -o hello.s
g++ -std=c++17 -c hello.cpp -o hello.o
g++ hello.o -o hello
./hello
```

산출물의 역할을 비교한다.

| 파일 | 역할 |
|---|---|
| `hello.cpp` | 직접 작성한 C++ source file |
| `hello.i` | GCC `-E`로 저장한 preprocessing 결과 |
| `hello.s` | GCC `-S`로 저장한 assembly source |
| `hello.o` | assembly까지 끝났지만 아직 link되지 않은 object file |
| `hello` | object file을 link해 만든 Linux executable |

`file hello.cpp hello.i hello.s hello.o hello`와 `wc -l hello.cpp hello.i hello.s`도 차이를 관찰하는 데 사용할 수 있다.

## 10. 자주 하는 실수

- **translation unit을 `.cpp` 파일과 동일시한다:** preprocessing 결과가 반영된 개념적 단위다.
- **header를 항상 독립 compile 대상으로 본다:** 보통 include되어 translation unit 형성에 참여한다.
- **`-S`가 assembly까지 끝낸다고 생각한다:** assembly source를 만들고 assembler 실행 전 멈춘다.
- **`-c`가 executable을 만든다고 생각한다:** linking을 생략하고 object file을 만든다.
- **object file을 직접 실행한다:** `hello.o`는 linker의 입력이지 최종 executable이 아니다.
- **표준이 `.i`, `.s`, `.o`, ELF를 규정한다고 생각한다:** 이들은 GCC/Linux에서 관찰하는 구현 요소다.
- **compiler 내부까지 한꺼번에 암기한다:** 현재는 단계별 입력·처리·출력 구분이 우선이다.

## 11. 핵심 정리

- GCC build는 preprocessing, compilation proper, assembly, linking으로 구분할 수 있다.
- `g++`는 관련 toolchain 단계를 조정하는 GNU C++ compiler driver다.
- preprocessing은 include, macro, conditional compilation directive를 처리한다.
- source file은 preprocessing을 거쳐 translation unit 형성에 참여한다.
- compilation proper는 C++ 문법과 의미를 분석하고 assembly code를 만든다.
- assembler는 assembly code를 object file로 만든다.
- object file은 아직 최종 executable이 아니다.
- linker는 object file과 필요한 구성 요소를 연결해 executable을 만든다.
- C++ 표준의 translation 개념과 GCC/Linux의 파일·명령 관례를 구분한다.

## 12. 확인 문제

1. source file과 translation unit은 왜 같은 개념이 아닌가?
2. `#include`와 `#define`은 어느 단계에서 처리되는가?
3. `g++ -S`와 `g++ -c`의 최종 출력은 어떻게 다른가?
4. `hello.o`를 아직 executable이라고 부를 수 없는 이유는 무엇인가?
5. source file이 하나뿐이어도 linking이 필요한 이유는 무엇인가?
6. C++ 표준과 GCC/Linux 구현에서 각각 설명하는 대상을 구분하라.

## 참고 자료

- [cppreference — Phases of translation](https://en.cppreference.com/w/cpp/language/translation_phases.html)
- [GCC — Overall Options](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html)
- [GCC — Preprocessor Options](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html)

## 다음 Step

다음은 **Step 0-3 — g++ 표준 선택과 -Wall -Wextra -pedantic -g**이다.

현재 Step의 실습은 [Step 0-2 Exercise](../../exercises/00-toolchain-and-build/0-2/README.md)에서 수행한다.
