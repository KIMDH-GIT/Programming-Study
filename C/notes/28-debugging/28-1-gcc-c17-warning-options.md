# 28-1. `-std=c17 -Wall -Wextra -Wpedantic`

## 1. 학습 목표

이번 Step에서는 GCC로 C 프로그램을 컴파일할 때 사용하는 다음 옵션의 역할을 구분한다.

```sh
-std=c17
-Wall
-Wextra
-Wpedantic
-Werror
```

학습이 끝나면 다음을 설명할 수 있어야 한다.

- `-std=c17`이 **C 언어 dialect를 선택하는 옵션**이라는 것을 설명할 수 있다.
- `-Wall`, `-Wextra`, `-Wpedantic`이 각각 어떤 종류의 diagnostic을 활성화하는지 구분할 수 있다.
- `-Wall`이 이름과 달리 **모든 warning을 활성화하지 않는다**는 것을 설명할 수 있다.
- `-Werror`가 warning을 error로 취급하는 GCC 옵션임을 설명할 수 있다.
- warning 없이 컴파일되었다는 사실과 프로그램의 correctness를 구분할 수 있다.

---

## 2. 선수 지식

이 Step을 이해하려면 다음 내용을 알고 있어야 한다.

- Part 0의 C translation 과정
  - preprocessing
  - compilation
  - assembly
  - linking
- source file과 executable의 차이
- compiler diagnostic의 의미
- compile-time error와 runtime error의 차이
- Undefined Behavior의 기본 개념

특히 다음 관계를 기억한다.

```text
C source code
    |
    v
Preprocessing
    |
    v
Compilation
    |
    |  <-- warning / error 등의 diagnostic 발생 가능
    v
Assembly
    |
    v
Linking
    |
    v
Executable
```

이번 Step의 옵션들은 주로 **translation 과정에서 compiler가 source code를 어떤 규칙으로 해석하고 어떤 diagnostic을 출력할 것인지**에 영향을 준다.

---

## 3. 핵심 개념

### 3.1 `-std=c17`

```sh
-std=c17
```

은 GCC가 source code를 해석할 때 사용할 **C language dialect**를 ISO C17 계열로 선택하도록 한다.

예를 들어

```sh
gcc -std=c17 main.c
```

라고 하면 GCC는 C17을 기준으로 source code를 처리한다.

중요한 점은 다음과 같다.

> `-std=c17`은 프로그램이 완전히 올바르다는 것을 증명하는 옵션이 아니다.

즉,

```text
-std=c17 사용
        ↓
C17 dialect 선택
```

이지,

```text
-std=c17 사용
        ↓
프로그램 correctness 보장
```

이 아니다.

프로그램에는 여전히 다음 문제가 존재할 수 있다.

- 잘못된 알고리즘
- 잘못된 입력 처리
- 메모리 접근 오류
- Undefined Behavior
- 논리 오류
- runtime error

---

### 3.2 `-Wall`

```sh
-Wall
```

은 GCC가 제공하는 여러 warning 중 **일반적으로 유용하고 비교적 쉽게 피할 수 있다고 판단되는 warning 집합**을 활성화한다.

이름만 보면 다음처럼 생각하기 쉽다.

```text
-Wall
=
Warning All
=
모든 warning 활성화
```

하지만 이것은 정확하지 않다.

실제로는

```text
-Wall
=
GCC가 정한 특정 warning 집합 활성화
```

이다.

예를 들어 compiler version에 따라 다음과 같은 여러 warning이 포함될 수 있다.

```text
unused variable
unused function
uninitialized use
return 관련 문제
format 관련 문제
```

단, 정확히 어떤 warning이 포함되는지는 **사용 중인 GCC version의 공식 문서**를 확인해야 한다.

---

### 3.3 `-Wextra`

```sh
-Wextra
```

는 `-Wall`에 포함되지 않은 **추가 warning 집합**을 활성화한다.

일반적으로 다음처럼 함께 사용한다.

```sh
gcc -Wall -Wextra main.c
```

관계는 다음처럼 이해하면 된다.

```text
GCC warning 전체
├── -Wall이 활성화하는 일부 warning
├── -Wextra가 추가로 활성화하는 일부 warning
└── 그 외 별도로 지정해야 하는 warning
```

따라서 다음 설명은 틀리다.

```text
-Wall이 모든 warning을 활성화한다.
```

그리고 다음 설명도 틀리다.

```text
-Wextra는 -Wall과 완전히 동일하다.
```

`-Wextra`는 이름 그대로 `-Wall`에 더해 사용하는 **추가 diagnostic 집합**이다.

---

### 3.4 `-Wpedantic`

```sh
-Wpedantic
```

은 선택한 language standard를 기준으로 **ISO C에서 요구하는 diagnostic이나 일부 non-standard extension 사용에 대해 더 엄격한 warning을 요청하는 옵션**이다.

예를 들어

```sh
gcc -std=c17 -Wpedantic main.c
```

라고 하면 C17을 기준으로 보다 엄격하게 source code를 검사한다.

하지만 다음처럼 이해하면 안 된다.

```text
-Wpedantic
=
모든 non-standard code를 반드시 compile error로 거부
```

`-Wpedantic` 자체는 기본적으로 **warning을 요청하는 옵션**이다.

따라서 어떤 코드에 대해 diagnostic이 발생하더라도 compilation이 계속될 수 있다.

---

### 3.5 `-Werror`

```sh
-Werror
```

는 발생한 warning을 **error처럼 취급**하게 한다.

예를 들어

```sh
gcc -Wall -Wextra -Wpedantic -Werror main.c
```

에서 warning이 하나 발생하면 build가 실패한다.

관계를 단순화하면 다음과 같다.

```text
-Wall / -Wextra / -Wpedantic
        |
        v
warning 발생
        |
        v
-Werror 사용?
    /       \
   No       Yes
   |         |
compile    warning을
계속 가능   error 취급
```

`-Werror`는 ISO C17 standard가 요구하는 규칙이 아니다.

GCC가 제공하는 compiler option이다.

따라서

```text
C17 requirement
```

와

```text
GCC build policy
```

를 구분해야 한다.

---

### 3.6 다섯 옵션의 역할 비교

| 옵션 | 역할 | 핵심 |
|---|---|---|
| `-std=c17` | C language dialect 선택 | C17 기준으로 source 해석 |
| `-Wall` | 주요 warning 집합 활성화 | 모든 warning은 아님 |
| `-Wextra` | 추가 warning 집합 활성화 | `-Wall` 보완 |
| `-Wpedantic` | 표준 준수 관련 diagnostic 강화 | extension 등을 더 엄격히 검사 |
| `-Werror` | warning을 error로 취급 | warning 발생 시 build 실패 가능 |

가장 중요한 구분은 다음이다.

```text
-std=c17
    ↓
"어떤 C 규칙으로 해석할 것인가?"

-Wall
-Wextra
-Wpedantic
    ↓
"어떤 문제를 warning으로 보고할 것인가?"

-Werror
    ↓
"발생한 warning을 build 실패로 처리할 것인가?"
```

---

## 4. 문법

기본적인 strict warning build는 다음과 같이 작성할 수 있다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o app
```

각 부분을 분해하면 다음과 같다.

```text
gcc
│
├── -std=c17
│      └── C17 dialect 선택
│
├── -Wall
│      └── 주요 warning 활성화
│
├── -Wextra
│      └── 추가 warning 활성화
│
├── -Wpedantic
│      └── 표준 준수 관련 diagnostic 강화
│
├── main.c
│      └── 입력 source file
│
├── -o
│      └── output 이름 지정
│
└── app
       └── 생성할 executable 이름
```

학습이나 CI 환경에서는 warning을 놓치지 않기 위해 `-Werror`를 추가할 수 있다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o app
```

하지만 `-Werror`를 사용하는 경우 compiler version이 변경되면서 새 warning이 추가되면 기존 source code의 build가 실패할 수 있다.

따라서

```text
-Werror
=
프로그램 correctness 보장
```

이 아니라

```text
-Werror
=
warning이 존재하는 build를 허용하지 않는 build policy
```

라고 이해해야 한다.

---

## 5. 최소 코드 예제

다음 프로그램은 세 정수의 합을 계산한다.

```c
#include <stdio.h>

static int sum(const int values[], size_t count)
{
    int result = 0;

    for (size_t i = 0; i < count; ++i) {
        result += values[i];
    }

    return result;
}

int main(void)
{
    int values[] = {1, 2, 3};

    printf("%d\n",
           sum(values, sizeof values / sizeof values[0]));

    return 0;
}
```

컴파일한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

실행한다.

```sh
./warning_options
```

출력은 다음과 같다.

```text
6
```

### 옵션을 단계적으로 적용하기

먼저 아무 warning option 없이 컴파일한다.

```sh
gcc main.c -o warning_options
```

다음으로 C17 dialect를 명시한다.

```sh
gcc -std=c17 main.c -o warning_options
```

`-Wall`을 추가한다.

```sh
gcc -std=c17 -Wall main.c -o warning_options
```

`-Wextra`를 추가한다.

```sh
gcc -std=c17 -Wall -Wextra main.c -o warning_options
```

`-Wpedantic`을 추가한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o warning_options
```

마지막으로 warning을 build failure로 처리한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

이렇게 단계적으로 실행하면 각 option이 서로 다른 역할을 한다는 것을 확인하기 쉽다.

---

## 6. 코드 해석

먼저 표준 입출력 함수를 사용하기 위해 다음 header를 포함한다.

```c
#include <stdio.h>
```

다음 함수는 정수 배열의 원소 합을 계산한다.

```c
static int sum(const int values[], size_t count)
```

`values`는 배열의 첫 번째 원소를 가리키는 pointer 형태의 parameter이며, `count`는 처리할 원소 개수이다.

```c
int result = 0;
```

합을 저장할 변수를 `0`으로 초기화한다.

```c
for (size_t i = 0; i < count; ++i) {
    result += values[i];
}
```

`i`가

```text
0
1
2
...
count - 1
```

범위를 순회하면서 각 배열 원소를 `result`에 더한다.

```c
return result;
```

최종 합을 반환한다.

`main()`에서는 다음 배열을 생성한다.

```c
int values[] = {1, 2, 3};
```

배열의 전체 크기를 한 원소의 크기로 나누어 원소 개수를 계산한다.

```c
sizeof values / sizeof values[0]
```

현재 배열에서는

```text
sizeof values
=
3 * sizeof(int)
```

이므로

```text
sizeof values / sizeof values[0]
=
3
```

이다.

따라서

```c
sum(values, 3)
```

과 같은 효과가 발생하며 결과는

```text
1 + 2 + 3 = 6
```

이다.

프로그램이

```sh
-std=c17
-Wall
-Wextra
-Wpedantic
-Werror
```

조건에서 warning 없이 build되었다고 하더라도 이것만으로 다음이 증명되는 것은 아니다.

```text
모든 입력에서 올바른 결과를 낸다.
메모리 오류가 절대 없다.
Undefined Behavior가 절대 없다.
알고리즘이 요구사항에 맞다.
```

compiler warning은 correctness proof가 아니라 **문제 가능성을 발견하기 위한 diagnostic mechanism**이다.

---

## 7. 내부 동작

### `[C17]`

ISO C standard는 다음과 같은 언어 자체의 규칙을 정의한다.

```text
syntax
constraints
semantics
object
type
expression
statement
translation
diagnostic requirements
```

하지만 다음 GCC command-line option 자체를 정의하지는 않는다.

```text
-Wall
-Wextra
-Wpedantic
-Werror
```

이들은 GCC가 제공하는 compiler interface이다.

---

### `[GCC]`

GCC는 command-line option에 따라 source code를 분석한다.

예를 들어

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c
```

에서는 대략 다음 관계가 형성된다.

```text
source code
    |
    v
GCC frontend
    |
    ├── C17 dialect 기준으로 parsing / semantic analysis
    |
    ├── -Wall diagnostic 검사
    |
    ├── -Wextra diagnostic 검사
    |
    └── -Wpedantic diagnostic 검사
            |
            v
      warning / error 출력
```

정확한 warning 종류와 diagnostic wording은 GCC version에 따라 달라질 수 있다.

예를 들어

```sh
gcc --version
```

으로 현재 version을 확인할 수 있다.

---

### `[OS / ABI / CPU]`

warning option은 기본적으로 translation-time diagnostic에 관계한다.

즉,

```text
-Wall
-Wextra
-Wpedantic
```

을 사용했다고 해서 자동으로 runtime safety check가 machine code에 추가되는 것은 아니다.

전체 층을 구분하면 다음과 같다.

```text
C17
    ↓
source language semantics

GCC options
    ↓
translation / diagnostic policy

ABI
    ↓
function call, register usage, binary interface

OS
    ↓
process 실행 및 resource 관리

CPU
    ↓
machine instruction 실행
```

따라서 compiler warning option과 runtime behavior는 동일한 층의 개념이 아니다.

---

## 8. 자주 하는 실수

### 실수 1. `-Wall`을 모든 warning이라고 생각한다.

잘못된 설명:

```text
-Wall은 GCC의 모든 warning을 활성화한다.
```

올바른 설명:

```text
-Wall은 GCC가 정한 주요 warning 집합을 활성화한다.
```

---

### 실수 2. `-Wextra`가 `-Wall`에 이미 전부 포함되어 있다고 생각한다.

다음처럼 사용하는 이유가 있다.

```sh
-Wall -Wextra
```

두 option이 완전히 동일하지 않기 때문이다.

---

### 실수 3. `-Wpedantic`을 모든 extension을 반드시 거부하는 option이라고 생각한다.

`-Wpedantic`은 기본적으로 diagnostic을 요청한다.

compile rejection 여부와 동일한 개념이 아니다.

---

### 실수 4. `-Werror`를 C17 standard requirement라고 생각한다.

`-Werror`는 GCC option이다.

```text
ISO C standard
```

와

```text
compiler-specific build policy
```

를 구분해야 한다.

---

### 실수 5. warning이 없으면 프로그램이 올바르다고 생각한다.

다음 command가 성공했다고 하자.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c
```

이것이 의미하는 것은 대략

```text
현재 GCC가 활성화된 diagnostic 조건에서
보고할 warning을 발견하지 않았다.
```

는 것이다.

다음을 의미하지 않는다.

```text
프로그램에 bug가 없다.
Undefined Behavior가 없다.
모든 입력에서 올바르게 동작한다.
```

---

## 9. 필수 실습

다음 실습을 수행한다.

[28-1 exercise](../../exercises/28-debugging/28-1/README.md)

실습에서는 먼저 다음 program을 작성한다.

```c
#include <stdio.h>

static int sum(const int values[], size_t count)
{
    int result = 0;

    for (size_t i = 0; i < count; ++i) {
        result += values[i];
    }

    return result;
}

int main(void)
{
    int values[] = {1, 2, 3};

    printf("%d\n",
           sum(values, sizeof values / sizeof values[0]));

    return 0;
}
```

그리고 다음 순서로 compile한다.

```sh
gcc main.c -o warning_options
```

```sh
gcc -std=c17 main.c -o warning_options
```

```sh
gcc -std=c17 -Wall main.c -o warning_options
```

```sh
gcc -std=c17 -Wall -Wextra main.c -o warning_options
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o warning_options
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

마지막으로 실행한다.

```sh
./warning_options
```

예상 출력:

```text
6
```

그리고 다음 표를 직접 작성한다.

| 옵션 | 역할 | C standard 자체의 옵션인가? |
|---|---|---|
| `-std=c17` | | |
| `-Wall` | | |
| `-Wextra` | | |
| `-Wpedantic` | | |
| `-Werror` | | |

---

## 10. 추가 실습

### ★ Option을 하나씩 추가하기

다음 순서로 compile command를 실행한다.

```sh
gcc main.c -o warning_options
```

```sh
gcc -std=c17 main.c -o warning_options
```

```sh
gcc -std=c17 -Wall main.c -o warning_options
```

```sh
gcc -std=c17 -Wall -Wextra main.c -o warning_options
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o warning_options
```

각 단계에서

```text
추가한 option
option의 역할
warning 발생 여부
```

를 기록한다.

---

### ★★ GCC manual에서 `-Wall` 확인하기

현재 사용 중인 GCC version을 확인한다.

```sh
gcc --version
```

그리고 GCC 공식 manual의 Warning Options 문서에서 `-Wall`이 어떤 warning들을 활성화하는지 일부 확인한다.

중요한 것은 목록 전체를 암기하는 것이 아니라

```text
-Wall != 모든 GCC warning
```

이라는 점을 확인하는 것이다.

---

### ★★★ Required diagnostic과 optional warning 구분하기

다음 두 개념을 비교한다.

```text
ISO C standard가 diagnostic을 요구하는 상황
```

과

```text
GCC가 추가적인 code quality 문제를 warning하는 상황
```

둘은 동일하지 않다.

compiler warning은 C standard가 요구하는 diagnostic보다 훨씬 넓은 범위의 문제를 보고할 수 있다.

---

## 11. 확인 문제

### 1. `-std=c17`은 무엇을 선택하는가?

GCC가 source code를 해석할 때 사용할 C language dialect를 C17 계열로 선택한다.

---

### 2. `-Wall`이 모든 warning을 뜻하지 않는 이유는?

`-Wall`은 GCC가 정의한 특정 warning 집합만 활성화하기 때문이다. GCC가 제공하는 모든 warning option이 포함되는 것은 아니다.

---

### 3. `-Wextra`의 역할은?

`-Wall`에 포함되지 않은 추가 warning 집합을 활성화한다.

---

### 4. `-Wpedantic`과 compile rejection은 왜 같은 말이 아닌가?

`-Wpedantic`은 선택한 language standard를 기준으로 추가 diagnostic을 요청하는 warning option이기 때문이다. warning이 발생했다고 해서 반드시 compilation 자체가 중단되는 것은 아니다.

---

### 5. `-Werror`는 C17 requirement인가?

아니다.

`-Werror`는 warning을 error로 취급하도록 하는 GCC compiler option이며 ISO C17 requirement가 아니다.

---

## 12. 핵심 정리

다음 세 층을 구분하는 것이 가장 중요하다.

```text
1. Language dialect

-std=c17
    ↓
C17을 기준으로 source 해석
```

```text
2. Diagnostic selection

-Wall
-Wextra
-Wpedantic
    ↓
어떤 문제를 warning으로 보고할지 결정
```

```text
3. Build policy

-Werror
    ↓
warning을 error처럼 취급
```

따라서 다음 command

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c
```

는

```text
C17을 기준으로 코드를 해석하고
여러 warning 검사를 활성화하고
warning이 발생하면 build를 실패시킨다.
```

는 의미이다.

하지만 이것은

```text
프로그램이 완전히 올바르다는 증명
```

은 아니다.

---

## 13. 다음 Step

[28-2. compiler warning 읽고 수정하기](28-2-reading-and-fixing-compiler-warnings.md)

다음 Step에서는 실제 warning message를 읽고

```text
warning 위치 찾기
→ warning 종류 확인
→ 원인 분석
→ source 수정
→ 다시 compile
```

하는 과정을 학습한다.

---

## 14. 참고 자료

- N1570, ISO/IEC 9899:201x Committee Draft, §5.1.1.3 Diagnostics
  - N1570은 C11 공개 Committee Draft이다.
  - diagnostic requirement의 기본 구조는 이후 C17에서도 유지된다.
- GCC Online Documentation — Warning Options
  - https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html
- GCC Online Documentation — C Dialect Options
  - https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html
