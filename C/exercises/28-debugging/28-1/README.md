# 28-1 실습: GCC C17 warning options

이론: [note](../../../notes/28-debugging/28-1-gcc-c17-warning-options.md)

## 실습 목적

이번 실습에서는 GCC의 다음 option을 실제 compile command에 적용한다.

```sh
-std=c17
-Wall
-Wextra
-Wpedantic
-Werror
```

실습의 핵심은 각 option을 단순히 암기하는 것이 아니라 다음 세 역할을 구분하는 것이다.

```text
Language dialect
    ↓
-std=c17

Diagnostic selection
    ↓
-Wall
-Wextra
-Wpedantic

Build policy
    ↓
-Werror
```

실습 완료 후 다음을 설명할 수 있어야 한다.

- `-std=c17`이 C17 language dialect를 선택한다.
- `-Wall`이 모든 warning을 의미하지 않는다.
- `-Wextra`가 `-Wall`에 추가되는 warning 집합이다.
- `-Wpedantic`이 표준 준수 관련 diagnostic을 강화한다.
- `-Werror`가 warning을 error처럼 처리한다.
- warning 없이 build되었다고 해서 프로그램 correctness가 증명되는 것은 아니다.

---

## 작성할 파일

다음 파일을 작성한다.

```text
main.c
```

최종 directory 예시는 다음과 같다.

```text
28-1/
├── README.md
└── main.c
```

`main.c`에는 세 정수

```text
1
2
3
```

의 합을 계산하여

```text
6
```

을 출력하는 프로그램을 작성한다.

다음 코드를 사용한다.

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

---

## 해야 할 일

### 1. `main.c` 작성

다음 조건을 만족하도록 작성한다.

```text
정수 배열 생성
→ 배열 길이 계산
→ sum() 함수 호출
→ 합 출력
```

배열:

```c
int values[] = {1, 2, 3};
```

배열 길이:

```c
sizeof values / sizeof values[0]
```

예상 계산:

```text
1 + 2 + 3 = 6
```

---

### 2. option 없이 compile

먼저 warning option을 추가하지 않고 compile한다.

```sh
gcc main.c -o warning_options
```

이 command를 기준점으로 사용한다.

---

### 3. `-std=c17` 추가

```sh
gcc -std=c17 main.c -o warning_options
```

확인할 내용:

```text
-std=c17
→ GCC가 C17 language dialect를 사용하도록 지정
```

이 option은 warning option과 역할이 다르다.

---

### 4. `-Wall` 추가

```sh
gcc -std=c17 -Wall main.c -o warning_options
```

확인할 내용:

```text
-Wall
→ GCC가 정한 주요 warning 집합 활성화
```

다음처럼 설명하면 안 된다.

```text
-Wall = 모든 warning
```

---

### 5. `-Wextra` 추가

```sh
gcc -std=c17 -Wall -Wextra main.c -o warning_options
```

확인할 내용:

```text
-Wextra
→ -Wall에 포함되지 않은 추가 warning 집합 활성화
```

---

### 6. `-Wpedantic` 추가

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o warning_options
```

확인할 내용:

```text
-Wpedantic
→ 선택한 C standard를 기준으로
   표준 준수 관련 diagnostic 강화
```

`-Wpedantic` 자체를

```text
모든 non-standard code를 무조건 compile error로 만든다.
```

라고 설명하지 않는다.

---

### 7. `-Werror` 추가

마지막으로 다음 command를 사용한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

확인할 내용:

```text
-Werror
→ 발생한 warning을 error처럼 처리
→ warning이 존재하면 build 실패 가능
```

---

### 8. 프로그램 실행

```sh
./warning_options
```

출력을 확인한다.

```text
6
```

---

### 9. option 역할 표 작성

README 또는 별도의 실습 기록에 다음 표를 작성한다.

| 옵션 | 분류 | 역할 |
|---|---|---|
| `-std=c17` | Language dialect | GCC가 C17 dialect를 사용하도록 지정 |
| `-Wall` | Warning option | 주요 warning 집합 활성화 |
| `-Wextra` | Warning option | 추가 warning 집합 활성화 |
| `-Wpedantic` | Warning option | 표준 준수 관련 diagnostic 강화 |
| `-Werror` | Build policy | warning을 error처럼 처리 |

---

## 사용할 개념

### `-std=c17`

```sh
-std=c17
```

GCC가 C17 language dialect를 사용하도록 지정한다.

핵심 질문:

```text
이 source code를 어떤 C language 규칙으로 해석할 것인가?
```

---

### `-Wall`

```sh
-Wall
```

GCC가 정한 주요 warning 집합을 활성화한다.

주의:

```text
-Wall != 모든 GCC warning
```

---

### `-Wextra`

```sh
-Wextra
```

`-Wall`에 포함되지 않은 추가 warning 집합을 활성화한다.

일반적으로 다음처럼 함께 사용한다.

```sh
-Wall -Wextra
```

---

### `-Wpedantic`

```sh
-Wpedantic
```

선택한 language standard를 기준으로 표준 준수 관련 diagnostic을 강화한다.

현재 실습에서는

```sh
-std=c17 -Wpedantic
```

이므로 C17을 기준으로 검사한다.

---

### `-Werror`

```sh
-Werror
```

warning을 error처럼 취급한다.

즉,

```text
warning 발생
    ↓
-Werror 존재
    ↓
build 실패
```

가 될 수 있다.

---

## 컴파일 방법

이번 실습에서는 option을 한 번에 입력하기 전에 단계적으로 추가해본다.

### 단계 1

```sh
gcc main.c -o warning_options
```

### 단계 2

```sh
gcc -std=c17 main.c -o warning_options
```

### 단계 3

```sh
gcc -std=c17 -Wall main.c -o warning_options
```

### 단계 4

```sh
gcc -std=c17 -Wall -Wextra main.c -o warning_options
```

### 단계 5

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o warning_options
```

### 단계 6

최종 compile command:

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

각 단계를 실행하면서 다음을 기록한다.

```text
1. 새로 추가한 option
2. option의 역할
3. warning 발생 여부
4. build 성공 여부
```

---

## 실행 방법

compile에 성공했다면 다음 command로 실행한다.

```sh
./warning_options
```

Linux / WSL shell에서는 현재 directory의 executable을 실행하기 위해 앞에

```text
./
```

를 붙인다.

따라서

```sh
./warning_options
```

은

```text
현재 directory에 존재하는 warning_options executable을 실행한다.
```

는 의미이다.

---

## 예상 관찰 결과

최종 command:

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

를 실행했을 때 정상적으로 작성된 source라면 warning 없이 build된다.

실행하면

```sh
./warning_options
```

다음 결과가 출력된다.

```text
6
```

option을 단계적으로 추가했을 때 이번 정상 예제에서는 출력 결과 자체는 변하지 않는다.

```text
gcc main.c
        ↓

gcc -std=c17 main.c
        ↓

gcc -std=c17 -Wall main.c
        ↓

gcc -std=c17 -Wall -Wextra main.c
        ↓

gcc -std=c17 -Wall -Wextra -Wpedantic main.c
        ↓

gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c
```

모두 정상적인 source라면 실행 결과는

```text
6
```

이다.

차이는 실행 결과가 아니라 **compiler가 source를 해석하고 diagnostic을 처리하는 방식**에 있다.

---

## 확인 포인트

실습 완료 후 다음 질문에 답할 수 있어야 한다.

### 1. `-std=c17`은 warning option인가?

아니다.

C language dialect를 선택하는 option이다.

---

### 2. `-Wall`은 모든 warning을 활성화하는가?

아니다.

GCC가 정한 특정 warning 집합을 활성화한다.

---

### 3. 왜 `-Wall -Wextra`를 함께 사용하는가?

`-Wextra`가 `-Wall`에 포함되지 않은 추가 warning을 활성화하기 때문이다.

---

### 4. `-Wpedantic`은 무엇을 기준으로 동작하는가?

선택된 language standard를 기준으로 동작한다.

이번 실습에서는

```sh
-std=c17
```

을 사용하므로 C17을 기준으로 한다.

---

### 5. `-Werror`의 역할은 무엇인가?

warning을 error처럼 취급하여 warning이 발생한 build가 성공하지 못하도록 할 수 있다.

---

### 6. 다음 command가 성공하면 프로그램이 완전히 올바른가?

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c
```

아니다.

이 command의 성공은 현재 활성화된 GCC diagnostic에서 문제가 보고되지 않았다는 의미일 뿐이다.

다음과 같은 문제는 여전히 존재할 수 있다.

```text
logic error
algorithm error
잘못된 요구사항 구현
특정 입력에서 발생하는 오류
compiler가 찾지 못한 Undefined Behavior
runtime resource 문제
```

따라서

```text
warning 0개 != correctness proof
```

이다.

---

## 추가 실습

### ★ Option을 하나씩 추가한다

다음 command를 순서대로 실행한다.

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

다음 표를 작성한다.

| 단계 | 추가 option | warning | build |
|---|---|---|---|
| 1 | 없음 | | |
| 2 | `-std=c17` | | |
| 3 | `-Wall` | | |
| 4 | `-Wextra` | | |
| 5 | `-Wpedantic` | | |
| 6 | `-Werror` | | |

---

### ★★ GCC manual의 warning 집합을 확인한다

먼저 GCC version을 확인한다.

```sh
gcc --version
```

그 다음 GCC 공식 Warning Options 문서에서 `-Wall` 항목을 찾는다.

확인할 내용은 다음과 같다.

```text
1. -Wall이 여러 warning option을 묶은 option인지
2. -Wall이 모든 warning을 포함하지 않는지
3. -Wextra가 별도로 존재하는지
```

목록 전체를 암기할 필요는 없다.

핵심은 다음이다.

```text
-Wall은 이름과 달리 모든 warning을 활성화하지 않는다.
```

---

### ★★★ Required diagnostic과 warning을 구분한다

다음 두 종류를 조사한다.

```text
A. ISO C standard가 diagnostic을 요구하는 경우
```

```text
B. GCC가 자체적으로 제공하는 optional warning
```

그리고 다음 차이를 정리한다.

| 구분 | 의미 |
|---|---|
| Required diagnostic | C standard가 implementation에 diagnostic을 요구 |
| Optional warning | compiler가 추가적인 문제 가능성을 알려줌 |

이를 통해

```text
C standard
```

와

```text
GCC diagnostic system
```

이 동일한 개념이 아님을 확인한다.

---

## 완료 기준

다음 항목을 모두 만족하면 실습을 완료한 것으로 본다.

- [ ] `main.c`를 작성했다.
- [ ] 세 정수 `1`, `2`, `3`의 합을 계산하도록 구현했다.
- [ ] 배열 길이를 `sizeof`를 사용하여 계산했다.
- [ ] option 없이 compile해보았다.
- [ ] `-std=c17`을 추가해서 compile했다.
- [ ] `-Wall`을 추가해서 compile했다.
- [ ] `-Wextra`를 추가해서 compile했다.
- [ ] `-Wpedantic`을 추가해서 compile했다.
- [ ] `-Werror`를 추가해서 compile했다.
- [ ] 최종 command로 warning 없이 build했다.
- [ ] 프로그램 실행 결과 `6`을 확인했다.
- [ ] `-std=c17`과 warning option의 역할을 구분할 수 있다.
- [ ] `-Wall`이 모든 warning을 의미하지 않는다고 설명할 수 있다.
- [ ] `-Wextra`의 역할을 설명할 수 있다.
- [ ] `-Wpedantic`의 역할을 설명할 수 있다.
- [ ] `-Werror`가 C17 requirement가 아님을 설명할 수 있다.
- [ ] warning 없이 build된 사실을 correctness proof라고 설명하지 않는다.

최종적으로 다음 command의 각 부분을 설명할 수 있어야 한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```

```text
gcc
    GCC compiler driver

-std=c17
    C17 language dialect 선택

-Wall
    주요 warning 집합 활성화

-Wextra
    추가 warning 집합 활성화

-Wpedantic
    표준 준수 관련 diagnostic 강화

-Werror
    warning을 error처럼 처리

main.c
    입력 source file

-o warning_options
    생성할 executable 이름 지정
```
