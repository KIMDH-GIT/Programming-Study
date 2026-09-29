# Step 0-1 Exercise — C++ 소스, 실행 파일, 최소 빌드

학습 문서: [Step 0-1 — C++ 소스, 실행 파일, 최소 빌드](../../../notes/00-toolchain-and-build/0-1-cpp-source-executable-and-minimal-build.md)

## 실습 목적

- 하나의 C++ source를 직접 작성하고 executable로 빌드한다.
- `-o`, 실행 경로, program output, exit status를 관찰한다.
- source 수정 전후와 rebuild 전후의 차이를 설명한다.

## 준비

이 exercise directory로 이동하고 `g++`를 확인한다.

```bash
cd exercises/00-toolchain-and-build/0-1
g++ --version
```

## 실습 1 — 최소 프로그램 작성과 build

`hello.cpp`를 만든다.

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, C++!\n";
    return 0;
}
```

build 전에 생성될 output 파일 이름을 예상한다.

- 예상 output:
- source와 output이 서로 다른 파일이어야 하는 이유:

다음 command로 build한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic hello.cpp -o hello
echo $?
```

## 실습 2 — `-o`와 실행 파일 확인

source와 build 결과를 조사한다.

```bash
ls -l hello.cpp hello
file hello.cpp hello
```

| 파일 | 역할 | `file` 출력의 핵심 표현 |
|---|---|---|
| `hello.cpp` |  |  |
| `hello` |  |  |

program을 실행하고 바로 exit status를 확인한다.

```bash
./hello
echo $?
```

- program output:
- exit status:
- build command의 출력과 program output의 차이:

## 실습 3 — Source 수정 전후 실행 비교

`hello.cpp`의 출력 문구를 다음과 같이 수정하고 저장한다.

```cpp
std::cout << "Source changed\n";
```

아직 rebuild하지 말고 `./hello`의 출력을 먼저 예측한다.

- rebuild 전 예상 출력:
- 그 출력을 예상한 이유:

실제로 실행한 뒤 다시 build하고 실행한다.

```bash
./hello
g++ -std=c++17 -Wall -Wextra -pedantic hello.cpp -o hello && ./hello
```

- rebuild 전 실제 출력:
- rebuild 후 실제 출력:
- 두 결과가 달라진 이유:

## 관찰 결과 기록

| 단계 | 입력 또는 command | 생성·관찰 결과 |
|---|---|---|
| source 저장 |  |  |
| build |  |  |
| executable 실행 |  |  |
| exit status 확인 |  |  |
| source 수정만 수행 |  |  |
| rebuild 후 실행 |  |  |

## 생각해 볼 문제

1. source file, executable, process를 각각 한 문장으로 구분하라.
2. `-o hello`에서 `hello`는 무엇인가?
3. build가 실패했는데 이전 `hello`가 남아 있다면 어떤 문제가 생길 수 있는가?
4. 현재 directory의 파일을 `hello`가 아니라 `./hello`로 실행하는 이유는 무엇인가?
5. program output이 예상과 같아도 exit status를 별도로 확인할 이유는 무엇인가?
6. `.cpp`와 ELF가 C++ 언어 자체의 필수 규칙이 아닌 이유를 설명하라.

## 힌트

- `$?`는 직전에 실행한 command의 종료 상태다.
- source를 저장한 시점과 executable을 마지막으로 build한 시점을 구분한다.
- `./`는 현재 directory를 나타내는 상대 경로다.

## 완료 체크

- [ ] `hello.cpp`를 직접 작성했다.
- [ ] 기본 C++17 option으로 warning 없이 build했다.
- [ ] source와 executable을 `file`로 비교했다.
- [ ] program output과 exit status를 기록했다.
- [ ] source 수정 후 rebuild 전후의 출력을 비교했다.
- [ ] source file, executable, process의 차이를 설명할 수 있다.
- [ ] build와 실행이 별개의 동작임을 설명할 수 있다.
