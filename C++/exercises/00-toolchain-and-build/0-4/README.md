# Step 0-4 Exercise — compiler warning·compile error·linker error·runtime error

학습 문서: [Step 0-4 — compiler warning·compile error·linker error·runtime error](../../../notes/00-toolchain-and-build/0-4-compiler-warning-compile-error-linker-error-and-runtime-error.md)

## 실습 목적

- 문제가 발견될 단계와 생성될 산출물을 먼저 예측한다.
- 실제 diagnostic, exit status, object/executable 유무로 분류를 확인한다.
- build 성공과 program의 논리적 정확성을 구분한다.

## 준비

이 exercise directory로 이동하고 compiler version을 기록한다.

```bash
cd exercises/00-toolchain-and-build/0-4
g++ --version
```

각 실습은 **예측 → 실행 → diagnostic·파일·상태 확인 → 이유 설명** 순서로 진행한다. 같은 이름의 이전 산출물은 먼저 제거한다.

## 실습 1 — Warning

`warning.cpp`를 작성한다.

```cpp
int main()
{
    int unused_value = 42;
    return 0;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic warning.cpp -o warning
echo $?
ls -l warning
./warning
echo $?
```

- 예측 / 관찰 — diagnostic, build·program status, executable, warning을 확인할 이유:

## 실습 2 — Compile Error

`compile_error.cpp`를 작성한다.

```cpp
int main()
{
    return missing_name;
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic compile_error.cpp -o compile_error
echo $?
ls -l compile_error
```

- 예측 / 관찰 — diagnostic, build status, executable, compile 단계인 근거:

`missing_name`을 `0`으로 수정한 뒤 다시 build하여 diagnostic과 산출물의 변화를 확인한다.

## 실습 3 — Linker Error

`linker_error.cpp`에는 declaration과 호출만 작성한다.

```cpp
int add(int a, int b);

int main()
{
    return add(1, 2);
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -c linker_error.cpp -o linker_error.o
echo $?
file linker_error.o
g++ linker_error.o -o linker_error
echo $?
ls -l linker_error
```

- 예측 / 관찰 — compile·link 상태, 산출물, linker 표현, `undefined reference`의 원인:

다음 `add.cpp`를 추가한다.

```cpp
int add(int a, int b) { return a + b; }
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -c add.cpp -o add.o
g++ linker_error.o add.o -o linker_fixed
./linker_fixed
echo $?
```

## 실습 4 — Runtime Problem

`runtime_problem.cpp`를 작성한다.

```cpp
#include <stdexcept>

int main()
{
    throw std::runtime_error("runtime example");
}
```

```bash
g++ -std=c++17 -Wall -Wextra -pedantic runtime_problem.cpp -o runtime_problem
echo $?
file runtime_problem
./runtime_problem
echo $?
```

- 예측 / 관찰 — build·program status, executable, 표준 보장과 환경별 message:

## 실습 5 — Logic Bug

`logic_bug.cpp`를 작성한다.

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

```bash
g++ -std=c++17 -Wall -Wextra -pedantic logic_bug.cpp -o logic_bug
echo $?
./logic_bug
echo $?
```

- 예측 / 관찰 — build·program status, 출력, bug인 이유, 최소 수정 결과:

## 비교표 작성

| 실험 | 발견 단계 | command 상태 | object file | executable | 실행 가능 여부 |
|---|---|---:|---|---|---|
| warning |  |  |  |  |  |
| compile error |  |  |  |  |  |
| linker error |  |  |  |  |  |
| runtime problem |  |  |  |  |  |
| logic bug |  |  |  |  |  |

## 생각해 볼 문제

1. warning이 있어도 executable이 만들어질 수 있는 이유는 무엇인가?
2. compile이 성공했는데 link가 실패할 수 있는 이유는 무엇인가?
3. `undefined reference`는 왜 보통 linker 단계 문제인가?
4. executable이 존재한다는 사실이 program이 정상이라는 뜻인가?
5. runtime에서 문제가 발생하면 반드시 현재 source의 compile과 link에 문제가 없었다고 말할 수 있는가?
6. logic bug는 왜 compiler가 잡지 못할 수 있는가?
7. undefined behavior와 runtime error를 같은 의미로 쓰면 왜 안 되는가?
8. `g++` 한 명령을 실행했는데도 compile error와 linker error가 서로 다른 이유는 무엇인가?

## 힌트

- `echo $?`는 반드시 확인할 command 바로 다음에 실행한다.
- `/usr/bin/ld`, `undefined reference`, `ld returned`는 linker 단계의 단서다.
- object file의 존재와 executable의 존재를 따로 확인한다.
- runtime message와 숫자 상태는 환경별이며 compiler는 이름만으로 의도를 알 수 없다.

## 완료 체크

- [ ] 다섯 source의 결과를 실행 전에 예측했다.
- [ ] warning과 compile error의 diagnostic·산출물 차이를 확인했다.
- [ ] compile-only 성공 뒤 linker error를 재현했다.
- [ ] definition 추가 후 link 성공과 non-zero program status를 구분했다.
- [ ] runtime build·실행 결과와 logic bug·undefined behavior를 구분했다.
