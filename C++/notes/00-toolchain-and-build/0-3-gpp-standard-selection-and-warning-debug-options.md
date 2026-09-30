# Step 0-3 — g++ 표준 선택과 -Wall -Wextra -pedantic -g

## 학습 목표

- compiler version과 C++ language standard를 구분한다.
- `-std=c++17`, `-Wall`, `-Wextra`, `-pedantic`, `-g`의 역할을 설명한다.
- warning과 error를 diagnostic의 종류로 이해하고 build 결과와 구분한다.
- 같은 source를 option별로 build하여 diagnostic 변화를 관찰한다.
- 일반 학습용 build와 debugging용 build 명령을 구분해 사용한다.

## 선수 지식

- Step 0-1의 source file, executable, build 개념
- Step 0-2의 preprocessing, compilation, assembly, linking 흐름
- Linux 또는 WSL에서 `g++` 명령을 실행하는 방법

## 1. Compiler Option이 필요한 이유

같은 `g++`도 command-line option에 따라 받아들이는 C++ 언어 규칙, 출력할 diagnostic, executable에 넣을 정보가 달라진다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

| option | 이 Step에서의 역할 |
|---|---|
| `-std=c++17` | C++17 language mode를 선택한다. |
| `-Wall` | GCC가 정한 주요 warning option 묶음을 활성화한다. |
| `-Wextra` | `-Wall`에 포함되지 않은 추가 warning 묶음을 활성화한다. |
| `-pedantic` | 선택한 표준을 기준으로 요구되는 진단과 일부 비표준 확장 사용 진단을 활성화한다. |
| `-g` | debugger가 사용할 debug information을 생성한다. |

이 option들은 한 가지 “엄격 모드”를 만드는 동일 계열의 switch가 아니다. 언어 모드 선택, diagnostic 제어, debug 정보 생성이라는 서로 다른 목적을 조합한다.

## 2. Compiler Version과 C++ Standard

먼저 현재 compiler 구현의 version을 확인한다.

```bash
g++ --version
```

`GCC 13.3.0`은 GNU Compiler Collection 구현의 version이고, `C++17`은 2017년에 정해진 C++ language standard의 한 revision이다.

GCC 13.3.0 하나로 `-std=c++11`, `-std=c++14`, `-std=c++17`, `-std=c++20`, `-std=c++23` 같은 여러 language mode를 선택할 수 있다. 반대로 여러 compiler version이 C++17을 지원할 수 있지만 지원 범위, diagnostic 문구, 구현 bug는 같다고 보장되지 않는다.

따라서 “GCC 13을 사용한다”와 “C++17로 compile한다”는 서로 바꿔 쓸 수 없는 문장이다.

## 3. `-std=c++17`

`-std=`는 compiler가 사용할 language standard 또는 dialect를 선택한다.

```bash
g++ -std=c++17 main.cpp -o main
```

이 교재는 기본적으로 C++17을 사용한다. 명령에 표준을 명시하면 compiler의 기본값이 version에 따라 달라져도 학습 기준을 유지할 수 있다.

### ISO mode와 GNU mode

GCC는 C++17에 대해 대표적으로 다음 두 mode를 제공한다.

| option | language mode |
|---|---|
| `-std=c++17` | ISO C++17을 기준으로 하는 mode |
| `-std=gnu++17` | C++17을 바탕으로 GNU extensions도 활성화하는 GNU dialect |

`gnu++17`도 C++17과 무관한 별도 표준은 아니다. C++17 기반 GNU dialect다. GCC 13.3.0에서는 `gnu++17`이 C++의 기본 mode지만, 이 교재는 표준 기준을 드러내기 위해 `-std=c++17`을 명시한다.

strict ISO mode라고 해서 GCC가 모든 extension을 즉시 error로 거부하는 것은 아니다. 일부 extension은 받아들이되 `-pedantic`을 함께 사용했을 때 진단한다.

## 4. Warning과 Error

compiler가 source에 대해 출력하는 message를 넓게 diagnostic이라고 부를 수 있으며, error와 warning은 그 종류다.

일반적으로 error는 compiler가 해당 build를 정상적으로 완료할 수 없다는 뜻이다. warning은 의심스러운 구성이나 실수 가능성을 알리지만, 보통 build는 계속되어 executable이 생성될 수 있다.

다음 관계를 혼동하지 않는다.

- compile 성공은 program의 정확성을 보장하지 않는다.
- warning이 발생한 code가 반드시 bug인 것은 아니다.
- warning이 없다고 bug가 없다는 뜻도 아니다.

예를 들어 잘못된 계산식은 문법과 type 규칙을 만족해 warning 없이 compile될 수 있다. 반대로 사용하지 않는 parameter는 interface를 맞추기 위해 의도적으로 남겨 둔 것일 수 있다.

warning은 판결문이 아니라 **programmer가 확인할 지점을 좁혀 주는 단서**다. 무시하지 말고 code의 의도와 비교해 원인을 판단한다.

## 5. `-Wall`

`-Wall`은 이름과 달리 GCC의 모든 warning을 활성화하지 않는다.

```bash
g++ -std=c++17 -Wall main.cpp -o main
```

GCC 문서에서 `-Wall`은 여러 사용자가 의심스럽다고 여기며 비교적 피하기 쉬운 구성에 관한 warning option들을 활성화하는 묶음이다. `-Wunused-variable`, `-Wreturn-type` 등 여러 개별 option이 포함된다.

모든 warning을 한꺼번에 켜지 않는 이유는 diagnostic마다 목적과 false positive 가능성, code에 적용하기 어려운 정도가 다르기 때문이다. 일부는 `-Wextra`에 있고, 일부는 필요에 따라 개별 option으로 켜야 한다.

따라서 `-Wall`은 “모든 warning”이 아니라 **GCC가 정의한 주요 warning 집합**이라고 설명해야 한다.

## 6. `-Wextra`

`-Wextra`는 `-Wall`에 포함되지 않은 추가 warning option들을 활성화한다.

```bash
g++ -std=c++17 -Wall -Wextra main.cpp -o main
```

둘을 함께 쓰는 이유는 `-Wextra`가 `-Wall`을 대체하는 상위 단계가 아니라, 서로 겹치는 부분이 있더라도 추가 진단을 제공하는 별도 묶음이기 때문이다.

GCC 13.3.0에서는 10절 실험 code의 사용하지 않는 parameter `input`이 `-Wall`만으로 진단되지 않았고, `-Wall -Wextra`에서 `warning: unused parameter ‘input’ [-Wunused-parameter]`로 진단되었다.

compiler version과 source 형태에 따라 세부 warning 집합은 달라질 수 있다. 어떤 option이 특정 diagnostic을 만드는지는 사용하는 GCC에서 직접 확인한다.

## 7. `-pedantic`

`-pedantic`은 단순히 “warning을 더 많이 켠다”는 뜻으로만 설명하면 부족하다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic main.cpp -o main
```

GCC 문서에 따르면 `-pedantic`은 strict ISO C++가 요구하는 warning을 내고, 금지된 extension 사용과 일부 ISO C++ 비준수 program을 진단한다. 판단 기준은 `-std=`로 선택한 standard다. GNU dialect를 선택한 경우에도 대응하는 ISO base standard가 기준이 된다.

다음 variable length array는 GCC가 C++ extension으로 받아들일 수 있지만 ISO C++17의 표준 배열 기능은 아니다.

```cpp
int main()
{
    int count = 3;
    int values[count]{};
    return values[0];
}
```

GCC 13.3.0에서 `-std=c++17 -Wall -Wextra`로는 이 extension에 관한 warning이 없었지만, `-pedantic`을 추가하면 다음 diagnostic이 발생했다.

```text
warning: ISO C++ forbids variable length array ‘values’ [-Wvla]
```

그래도 이 환경에서는 warning이므로 executable이 생성되었다. `-pedantic-errors`는 관련 required diagnostic을 error로 만드는 별도 option이며, 이번 Step의 기본 명령에는 사용하지 않는다.

`-pedantic`이 모든 비표준 동작이나 모든 program bug를 찾아내는 것도 아니다. 선택한 표준과 GCC가 제공하는 diagnostic 범위 안에서 해석해야 한다.

## 8. `-g`와 Debug Information

`-g`는 program을 자동으로 debugging하지 않는다. object file과 executable에 debug information을 생성하여 GDB 같은 debugger가 source line, function, variable 정보를 활용할 수 있게 한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

Linux의 일반적인 GCC 환경에서는 `file main` 또는 `readelf --sections main`으로 debug information의 존재를 관찰할 수 있다. GCC 13.3.0 실험에서는 `-g`를 넣은 executable에 `.debug_info`, `.debug_line` 등의 section이 생성되었다.

`-g`가 program의 논리를 수정하거나 breakpoint를 자동으로 설정하는 것은 아니다. debugger가 source 수준 정보를 사용할 수 있도록 build 산출물을 준비할 뿐이다. 실제 GDB 사용은 Step 0-5에서 다룬다.

### `-g`와 optimization은 다른 option이다

`-g`는 debug information을 생성하고 `-O...`는 optimization level을 선택한다. `-g`를 썼다고 optimization이 자동으로 꺼지는 것은 아니며 GCC는 둘을 함께 사용할 수 있다. 다만 optimized program에서는 일부 variable이나 실행 순서가 source와 다르게 보일 수 있다. optimization의 자세한 선택은 이번 Step에서 다루지 않는다.

## 9. Option을 조합한 기본 Build Command

일반 학습에서는 다음을 기본으로 사용한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic main.cpp -o main
```

source-level debugging을 준비할 때는 `-g`를 추가한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

option의 순서보다 각 option의 역할을 구분하는 것이 중요하다. `main.cpp`는 입력이고 `-o main`은 output 이름을 지정한다는 내용은 Step 0-1에서 배웠으므로 여기서는 반복하지 않는다.

## 10. 실제 Compiler Diagnostic 비교

다음 source는 GCC 13.3.0에서 option별 차이를 한 번에 관찰하도록 만든 실험용 code다.

```cpp
int inspect(int input)
{
    int unused_value = 10;
    int count = 3;
    int values[count]{};
    return values[0];
}

int main()
{
    return inspect(1);
}
```

같은 source에 option을 하나씩 추가했다.

| command의 option | GCC 13.3.0에서 추가로 관찰한 diagnostic | compile | executable |
|---|---|---|---|
| `-std=c++17` | 없음 | 성공 | 생성 |
| `-std=c++17 -Wall` | `unused_value`: `-Wunused-variable` | 성공 | 생성 |
| `-std=c++17 -Wall -Wextra` | `input`: `-Wunused-parameter` 추가 | 성공 | 생성 |
| `-std=c++17 -Wall -Wextra -pedantic` | variable length array: `-Wvla` 추가 | 성공 | 생성 |
| 위 option + `-g` | warning 종류는 동일, debug information 추가 | 성공 | 생성 |

이 표는 option의 일반 정의와 이 source에서의 실제 관찰을 함께 보여 준다. 모든 source가 같은 warning을 내는 것은 아니며, 다른 GCC version에서는 diagnostic 문구나 warning 집합이 달라질 수 있다.

마지막 두 executable을 `file`로 비교했을 때 `-g`를 사용한 쪽에는 `with debug_info`가 표시되었고, `readelf`에서는 debug section을 확인할 수 있었다. `-g`는 warning의 종류를 늘리기 위한 option이 아니라는 점도 함께 드러난다.

## 11. 자주 하는 실수

- **GCC version을 C++ standard라고 부른다:** 구현 version과 language mode는 별개다.
- **`-Wall`을 모든 warning이라고 설명한다:** 여러 주요 warning의 GCC 정의 묶음이다.
- **`-Wextra`만 쓰면 `-Wall`도 모두 포함된다고 생각한다:** 두 option을 함께 사용한다.
- **warning이 있으면 compile이 반드시 실패한다고 생각한다:** warning이 있어도 executable이 생성될 수 있다.
- **warning이 없으면 program이 정확하다고 생각한다:** compiler가 모든 논리 오류를 판단하지는 않는다.
- **`-pedantic`을 단순한 강도 조절로 본다:** 선택한 standard와 extension의 관계가 핵심이다.
- **`-g`를 debug 실행 mode라고 부른다:** debugger용 정보를 생성하는 build option이다.
- **`-g`가 optimization을 끈다고 생각한다:** 두 종류의 option은 독립적으로 선택할 수 있다.

## 12. 핵심 정리

- GCC version과 C++ language standard는 서로 다른 선택 축이다.
- `-std=c++17`은 ISO C++17 mode를 선택하며 `gnu++17`은 C++17 기반 GNU dialect다.
- `-Wall -Wextra`는 서로 보완하는 warning 묶음이고, warning 없음은 정확성의 증명이 아니다.
- `-pedantic`은 선택한 standard를 기준으로 required diagnostic과 extension 사용을 진단한다.
- `-g`는 debugger가 활용할 debug information을 만들며 optimization을 자동으로 끄지 않는다.

## 13. 확인 문제

1. GCC 13.3.0과 C++17은 각각 무엇을 나타내는가?
2. `-std=c++17`과 `-std=gnu++17`의 관계를 설명하라.
3. `-Wall`을 “모든 warning”이라고 설명하면 왜 정확하지 않은가?
4. `-Wall -Wextra`를 함께 사용하는 이유는 무엇인가?
5. warning이 발생했는데도 executable이 생성될 수 있는 이유는 무엇인가?
6. `-pedantic`은 어떤 기준으로 diagnostic을 내는가?
7. `-g`를 사용하면 debugger 없이도 bug가 자동으로 수정되는가?
8. `-g`와 optimization level은 어떤 관계인가?

## 참고 자료

- [GCC 13.3 — Options Controlling C Dialect](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc/C-Dialect-Options.html)
- [GCC 13.3 — Options to Request or Suppress Warnings](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc/Warning-Options.html)
- [GCC 13.3 — Options for Debugging Your Program](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc/Debugging-Options.html)
- [cppreference — History of C++](https://en.cppreference.com/w/cpp/language/history.html)

## 다음 Step

다음은 **Step 0-4 — compiler warning·compile error·linker error·runtime error**이다.

현재 Step의 실습은 [Step 0-3 Exercise](../../exercises/00-toolchain-and-build/0-3/README.md)에서 수행한다.
