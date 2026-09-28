# Step 0-2 Exercise — 전처리·컴파일·어셈블·링크와 translation unit

학습 문서: [Step 0-2 — 전처리·컴파일·어셈블·링크와 translation unit](../../../notes/00-toolchain-and-build/0-2-preprocessing-compilation-assembly-linking-and-translation-unit.md)

## 실습 목적

- 하나의 source에서 build 단계별 산출물을 만든다.
- preprocessing, compilation proper, assembly, linking의 입력과 출력을 구분한다.
- source file과 translation unit, object file과 executable의 차이를 설명한다.

## 준비

이 exercise directory로 이동해 `hello.cpp`를 만든다.

```cpp
#include <iostream>

#define MESSAGE "Hello, build stages!"

int main()
{
    std::cout << MESSAGE << '\n';
    return 0;
}
```

## 실습 1 — Preprocessing 결과 확인

먼저 `#include`와 `MESSAGE`가 결과에서 어떻게 보일지 예상한다.

- `hello.cpp`와 달라질 부분:
- 예상하는 이유:

preprocessing까지만 수행한다.

```bash
g++ -std=c++17 -E hello.cpp -o hello.i
```

`hello.i`에서 예제의 `main` 주변을 찾아 처음의 예측과 비교한 뒤, 다음 명령의 결과도 기록한다.

```bash
file hello.cpp hello.i
wc -l hello.cpp hello.i
```

## 실습 2 — Assembly 파일 생성

```bash
g++ -std=c++17 -S hello.cpp -o hello.s
file hello.s
```

- `hello.s`는 text인가, executable인가?
- 이 명령은 어느 단계 뒤에서 멈추는가?

Assembly instruction의 의미를 해석할 필요는 없다.

## 실습 3 — Object File 생성

```bash
g++ -std=c++17 -c hello.cpp -o hello.o
file hello.o
```

실행 전에 `./hello.o`가 최종 program처럼 동작할지 예상한다.

- 예상:
- `hello.s`와 `hello.o`의 역할 차이:

## 실습 4 — Object File Linking

object file을 executable로 link한다.

```bash
g++ hello.o -o hello
file hello
./hello
echo $?
```

- program output:
- exit status:
- `hello.o`에서 `hello`를 만들 때 추가된 단계:

## 실습 5 — 중간 산출물 비교

```bash
ls -lh hello.cpp hello.i hello.s hello.o hello
file hello.cpp hello.i hello.s hello.o hello
```

이름이나 크기만으로 역할을 판단하지 말고, 각 파일을 어느 command가 만들었고 다음 어느 단계의 입력이 되는지 연결한다.

## 관찰 결과 기록

| 파일 | 생성 command | 해당 단계 뒤의 산출물 | 다음 역할 |
|---|---|---|---|
| `hello.cpp` | 직접 작성 |  |  |
| `hello.i` |  |  |  |
| `hello.s` |  |  |  |
| `hello.o` |  |  |  |
| `hello` |  |  |  |

## 생각해 볼 문제

1. `hello.cpp`와 translation unit은 항상 같은 것인가?
2. `hello.o`와 executable의 차이는 무엇인가?
3. `g++ -c`를 사용하면 일반적인 build와 무엇이 달라지는가?
4. source file 하나뿐인 program에서도 linking 단계가 필요한 이유는 무엇인가?
5. header file의 내용은 일반적으로 어떻게 translation unit에 들어오는가?
6. C++ 표준이 `hello.o`나 ELF 형식을 요구한다고 말하면 왜 잘못인가?

## 힌트

- `-E`, `-S`, `-c`는 GCC가 어느 단계에서 멈출지를 정한다.
- `g++`는 C++ toolchain 전체 과정을 조정하는 driver다.
- translation unit은 preprocessing 뒤의 개념적 단위다.
- object file은 linker의 입력이지만 아직 최종 executable은 아니다.

## 완료 체크

- [ ] `hello.i`, `hello.s`, `hello.o`, `hello`를 차례로 생성했다.
- [ ] 각 command가 멈춘 단계를 설명할 수 있다.
- [ ] source file과 translation unit을 구분할 수 있다.
- [ ] assembly source와 object file을 구분할 수 있다.
- [ ] object file을 link해 executable을 만들고 실행했다.
- [ ] C++ 표준 개념과 GCC/Linux 산출물을 구분할 수 있다.
