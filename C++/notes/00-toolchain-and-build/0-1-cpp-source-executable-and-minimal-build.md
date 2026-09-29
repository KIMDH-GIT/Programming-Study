# Step 0-1 — C++ 소스, 실행 파일, 최소 빌드

## 학습 목표

- C++ source file, executable, process를 구분한다.
- `g++`로 하나의 `.cpp` 파일을 C++17 executable로 빌드한다.
- build, 실행, 프로그램 출력, exit status를 따로 확인한다.
- source를 수정한 뒤 rebuild가 필요한 이유를 설명한다.
- C++ 언어 규칙과 GCC/Linux에서 관찰하는 동작을 구분한다.

## 선수 지식

- Linux 또는 WSL terminal에서 `cd`, `ls`를 사용할 수 있다.
- text editor로 파일을 만들고 저장할 수 있다.
- C의 `main` 함수와 표준 출력을 학습했다.

## 1. Source file과 executable

C++ source file은 사람이 작성하는 program text다. 이 교재에서는 `.cpp` 확장자를 사용한다.

```text
hello.cpp
```

`.cpp`는 GCC가 입력 언어를 판단할 때 사용하는 일반적인 관례다. ISO C++ 표준이 모든 source file에 이 확장자를 강제하는 것은 아니다.

source file을 저장하는 것만으로는 실행할 program이 만들어지지 않는다. build를 거쳐 별도의 executable을 생성해야 한다.

| 대상 | 의미 |
|---|---|
| source file | 사람이 편집하는 C++ program text |
| executable | build 결과로 생성된 실행 가능한 파일 |
| process | 운영체제가 executable을 적재해 실행 중인 상태 |

Linux에서는 executable에 `.exe` 확장자가 필요하지 않다. `hello`라는 파일을 실행하면 process가 만들어지고, process가 종료된 뒤에도 `hello` 파일은 남는다.

`chmod +x hello.cpp`는 파일 권한만 바꿀 뿐 source를 executable로 번역하지 않는다.

구현 관점의 전체 흐름은 다음과 같다.

```text
source file
  → preprocessing
  → compilation
  → assembly
  → linking
  → executable
```

이번 Step에서는 이 이름과 전체 방향만 사용한다. 각 단계와 translation unit은 Step 0-2에서 학습한다.

## 2. 최소 C++ 프로그램

`hello.cpp`를 다음과 같이 작성한다.

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, C++!\n";
    return 0;
}
```

현재는 다음만 알면 충분하다.

- `#include <iostream>`은 표준 stream 출력에 필요한 선언을 제공한다.
- `main`은 program 실행이 시작되는 함수다.
- `std::cout`은 standard output에 문자열을 보낸다.
- `return 0`은 성공을 나타내는 종료 상태를 반환한다.

`iostream`, namespace, stream의 자세한 의미는 Part 1에서 다룬다.

## 3. `g++`로 build하기

```bash
g++ -std=c++17 -Wall -Wextra -pedantic hello.cpp -o hello
```

`g++`는 GNU C++ compiler driver다. C++ source를 처리하는 데 필요한 compiler, assembler, linker 등을 호출하며, 위 명령은 linking까지 수행해 `hello` executable을 만든다.

성공한 build는 보통 아무 message도 출력하지 않는다. build 직후 종료 상태와 산출물을 확인할 수 있다.

```bash
echo $?
ls -l hello
```

`echo $?`는 반드시 build command 바로 다음에 실행한다. 다른 command를 먼저 실행하면 그 command의 종료 상태를 보게 된다.

## 4. `-std`, warning option, `-o`

| 인자 | 역할 |
|---|---|
| `-std=c++17` | C++17 언어 표준 모드를 선택한다. |
| `-Wall` | 자주 필요한 warning 묶음을 활성화한다. |
| `-Wextra` | `-Wall`에 포함되지 않은 추가 warning을 활성화한다. |
| `-pedantic` | 선택한 표준과 충돌하는 확장 사용을 진단하게 한다. |
| `hello.cpp` | 입력 source file이다. |
| `-o hello` | 주 output 파일 이름을 `hello`로 지정한다. |

이 option들의 세부 범위는 Step 0-3에서 다룬다. 여기서는 교재 전체에서 사용할 기본 build 형태를 익힌다.

`-Wall`이 모든 warning을 의미하거나 `-pedantic`이 모든 문제를 검출하는 것은 아니다. warning이 없다는 사실도 program 전체가 정확하다는 증명은 아니다.

## 5. 프로그램 실행

source와 executable의 종류를 비교해 볼 수 있다.

```bash
file hello.cpp hello
```

일반적인 Linux/WSL 환경에서는 `hello.cpp`를 text로, `hello`를 ELF executable 계열로 식별한다. 정확한 출력 문구와 executable 형식은 환경에 따라 달라질 수 있다.

현재 directory의 executable은 다음처럼 실행한다.

```bash
./hello
```

예상 출력:

```text
Hello, C++!
```

`./`는 현재 directory를 나타낸다. `hello`만 입력하면 shell은 보통 `PATH`에서 command를 찾으므로 현재 directory의 파일을 실행한다고 보장할 수 없다.

## 6. Source 수정과 rebuild

build된 executable은 build 당시 source의 결과다. 이후 `hello.cpp`를 수정해도 기존 `hello`는 자동으로 바뀌지 않는다.

출력 문구를 다음과 같이 바꿔 저장해 보자.

```cpp
std::cout << "Source changed\n";
```

rebuild 전에 `./hello`를 실행하면 이전 출력이 나온다. 변경을 반영하려면 다시 build해야 한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic hello.cpp -o hello && ./hello
```

`&&` 오른쪽 command는 왼쪽 build가 성공한 경우에만 실행된다. 이렇게 하면 build 실패 뒤에 남아 있는 이전 executable을 실수로 실행하는 일을 줄일 수 있다.

## 7. Build, 실행, 출력, 종료 상태 구분

다음 네 결과는 서로 다르다.

| 관찰 대상 | 확인하는 것 |
|---|---|
| build 성공 | source로 executable을 만들 수 있었는가 |
| executable 실행 | 운영체제가 program을 시작할 수 있었는가 |
| program output | 실행 중 standard output에 무엇을 기록했는가 |
| exit status | process가 어떤 상태로 종료되었는가 |

실행 직후 exit status를 확인한다.

```bash
./hello
echo $?
```

이 예제는 `main`에서 `0`을 반환하므로 정상적으로 끝났다면 `0`이 출력된다. `$?`는 직전 command의 상태만 보존한다.

## 8. C와의 연결

**C에서는:** 보통 `.c` source를 `gcc`로 빌드하고 `printf` 같은 C standard library 기능을 사용했다.

**C++에서는:** 이 교재에서 `.cpp` source를 `g++`로 빌드하며, 현재 예제는 `std::cout`을 사용한다.

**공통점:** source와 executable은 별개이고, source 변경을 반영하려면 rebuild해야 한다.

**차이점:** C와 C++는 서로 다른 언어와 standard library를 가진다. 비슷한 GCC command 흐름을 공유한다고 해서 C++가 단순히 C에 기능을 추가한 언어라는 뜻은 아니다.

## 9. 자주 하는 실수

- **source를 직접 실행한다:** `./hello.cpp`가 아니라 build된 `./hello`를 실행한다.
- **`./`를 생략한다:** 현재 directory의 executable은 경로를 명시한다.
- **수정 후 rebuild하지 않는다:** 저장과 build는 별개의 동작이다.
- **build 실패 뒤 이전 파일을 실행한다:** `build && run` 형태를 사용한다.
- **`-o`를 실행 option으로 오해한다:** `-o hello`는 output 파일 이름을 지정한다.

## 10. 핵심 정리

- source file은 사람이 작성하는 text이고 executable은 build 산출물이다.
- process는 executable이 실행 중인 상태다.
- `g++`는 C++ build 과정을 조정하는 compiler driver다.
- 기본 명령은 `g++ -std=c++17 -Wall -Wextra -pedantic hello.cpp -o hello`이다.
- `./hello`는 현재 directory의 executable을 실행한다.
- source 변경은 rebuild한 뒤에 executable에 반영된다.
- build, 실행, 출력, exit status는 따로 확인한다.
- C++ 표준은 `g++` 명령이나 Linux executable 형식을 규정하지 않는다.

## 11. 확인 문제

1. `hello.cpp`, `hello`, 실행 중인 process의 차이를 설명하라.
2. `-o hello`는 무엇을 지정하는가?
3. source를 수정하고 rebuild하지 않으면 어떤 program이 실행되는가?
4. `hello` 대신 `./hello`를 사용하는 이유는 무엇인가?
5. build 성공과 program output은 어떻게 다른가?
6. C++ 표준이 `.cpp`, `g++`, ELF를 직접 규정한다고 말하면 왜 잘못인가?

## 참고 자료

- [GCC — Compiling C++ Programs](https://gcc.gnu.org/onlinedocs/gcc/Invoking-G_002b_002b.html)
- [GCC — Overall Options](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html)
- [cppreference — Phases of translation](https://en.cppreference.com/w/cpp/language/translation_phases.html)

## 다음 Step

다음은 **Step 0-2 — 전처리·컴파일·어셈블·링크와 translation unit**이다.

현재 Step의 실습은 [Step 0-1 Exercise](../../exercises/00-toolchain-and-build/0-1/README.md)에서 수행한다.
