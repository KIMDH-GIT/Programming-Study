# Step 1-2. `#include`와 표준 헤더

기준은 C17의 호스트 환경이다. 이 Step에서는 헤더가 제공하는 선언과 링크되는 라이브러리 구현을 구별한다.

## 1. 학습 목표

- `#include`가 전처리 지시문임을 설명한다.
- `<stdio.h>`의 선언과 라이브러리 구현을 구별한다.
- `<...>`와 `"..."` 헤더 이름 형식의 차이를 안다.
- 전처리 결과와 실행 결과를 구분해 관찰한다.

## 2. 선수 지식

[Step 1-1](1-1-first-program-main.md)의 `int main(void)`과 함수 호출 표기를 알고 있어야 한다. `gcc -E`가 전처리 뒤에 멈춘다는 사실도 사용한다.

## 3. 핵심 개념

`#include <stdio.h>`는 C 문장이 아니라 전처리 지시문이다. 전처리기는 지정한 헤더 내용을 이후 번역의 입력에 포함시킨다. 따라서 `#include`는 실행 중 파일을 읽거나 라이브러리를 가져오는 runtime import가 아니다.

`stdio.h`는 `puts` 같은 표준 입출력 함수의 **선언**을 제공한다. 선언은 함수의 이름, 매개변수, 반환형이라는 인터페이스 설명이며 함수의 기계 코드 구현이 아니다. 구현은 라이브러리 또는 다른 번역 단위에 있고, **[GCC 구현]** 일반적인 호스트 환경용 빌드에서는 링크 단계가 외부 참조를 C 라이브러리 구현과 연결한다.

`<stdio.h>`는 구현이 정한 시스템 헤더 검색 위치를 사용한다. `"local.h"`는 현재 파일 주변의 구현 정의 검색 경로를 먼저 고려할 수 있다. 표준 헤더에는 의도가 명확한 `<stdio.h>`를 쓴다.

## 4. 문법

```c
#include <표준-헤더>
#include "프로젝트-헤더"
```

| 표현 | 처리 시점 | 주된 용도 |
|---|---|---|
| `#include <stdio.h>` | 전처리 | 구현이 제공하는 표준 헤더 |
| `#include "local.h"` | 전처리 | 프로젝트 헤더 |
| `puts("text");` | 실행 중 | 선언된 함수 호출 |

전처리 지시문 끝에는 C 문장처럼 세미콜론을 붙이지 않는다. 헤더 이름은 문자열 리터럴이 아니다.

## 5. 최소 코드 예제

`include-stdio.c`에 저장한다.

```c
#include <stdio.h>

int main(void)
{
    puts("Header declarations matter.");
    return 0;
}
```

```sh
gcc -std=c17 -E include-stdio.c -o include-stdio.i
gcc -std=c17 -Wall -Wextra -pedantic include-stdio.c -o include-stdio && ./include-stdio
```

## 6. 코드 해석

| 줄/토큰 | 해석 |
|---|---|
| `#` | 전처리 지시문 표시 |
| `include` | 헤더 내용을 전처리 입력에 포함하라는 지시 |
| `<stdio.h>` | 표준 입출력 관련 선언을 얻을 헤더 이름 |
| `puts` | `stdio.h`가 선언하는 표준 함수 |
| `;` | `puts` 호출 문장의 끝; `#include` 줄에는 쓰지 않음 |

전처리기는 include를 처리하고, 컴파일러는 선언을 바탕으로 호출을 검사하며, 링크 단계는 외부 참조를 구현과 연결한다. 헤더를 포함했다고 라이브러리 구현 코드가 복사되는 것은 아니다.

## 7. 내부 동작

1. **[C 표준]** 번역 단계에서 `#include` 줄은 지정한 헤더 또는 소스 파일의 전체 내용으로 대체된다.
2. **[GCC 구현]** 전처리기는 `<stdio.h>`를 구성된 시스템 include 경로에서 찾는다.
3. **[C 표준]** 헤더의 선언은 호출 형식을 알리며 함수 본문은 아니다.
4. **[GCC 구현]** 링크 단계가 `puts` 외부 참조를 라이브러리 구현과 연결한다.
5. **[GNU/Linux 구현]** 동적 링크 환경에서는 실행 시작 시 동적 로더가 공유 라이브러리를 준비할 수 있다. 이것은 `#include`의 실행이 아니다.

## 8. 자주 하는 실수

- `#include`가 실행 중 라이브러리를 import한다고 생각한다.
- `stdio.h`가 `puts`의 구현 코드 전체라고 생각한다.
- `#include <stdio.h>;`처럼 세미콜론을 붙인다.
- `puts` 호출에서 `<stdio.h>`를 생략한다.
- `"stdio.h"`와 `<stdio.h>`가 모든 구현에서 같다고 생각한다.
- `.i`가 길다고 라이브러리 기계 코드가 복사됐다고 결론낸다.

## 9. 필수 실습

### 선언을 포함하고 전처리 결과 비교하기

- **목적:** `#include`의 전처리 역할과 `puts` 선언의 필요성을 관찰한다.
- **해야 할 일:** `exercises/01-program-structure/1-2/include-stdio.c`를 직접 작성한다. `-E`로 `.i`를 만들고 `puts`를 찾은 뒤 빌드·실행한다.
- **사용할 개념:** `#include <stdio.h>`, 전처리, 함수 선언, 표준 라이브러리, 링크.
- **예상되는 관찰 결과:** `.i`는 길어질 수 있고 `puts` 선언과 원래 호출을 찾을 수 있다. 전처리 자체는 프로그램을 실행하지 않는다.
- **확인 포인트:** include 줄에 세미콜론이 없는가? 선언과 구현을 다른 말로 설명했는가?

명령과 기록 양식은 [1-2 실습 README](../../exercises/01-program-structure/1-2/README.md)에 있다.

## 10. 추가 실습

- ★ `gcc -E -H include-stdio.c -o /dev/null`로 GCC의 포함 경로 표시를 관찰한다.
- ★★ `<stdio.h>`를 뺀 `missing-declaration.c`를 별도로 만들어 진단을 기록한다.
- ★★★ 큰따옴표와 꺾쇠 형식의 검색 차이를 말로 정리한다.

## 11. 확인 문제

1. `#include`는 실행 중에 처리되는가?
2. `<stdio.h>`는 `puts`에 대해 주로 무엇을 제공하는가?
3. `puts` 구현은 보통 어느 단계에서 연결되는가?
4. include 줄 뒤에 세미콜론을 붙여야 하는가?
5. 두 헤더 이름 형식의 검색 규칙은 모든 구현에서 같은가?
6. `gcc -E` 성공은 링크 성공도 뜻하는가?

<details>
<summary>정답과 해설</summary>

1. 아니다. 전처리에서 처리된다.
2. 함수 선언을 제공한다.
3. 링크 단계다.
4. 아니다.
5. 아니다. 경로와 순서는 구현 정의다.
6. 아니다. `-E`는 전처리 뒤에 멈춘다.

</details>

## 12. 핵심 정리

`#include`는 runtime import가 아닌 전처리 지시문이다. `<stdio.h>`는 함수 선언을 제공하고, 실제 구현은 별도 링크 대상이다. 표준 헤더에는 `<...>`를 쓴다.

## 13. 다음 Step

[Step 1-3. statement·중괄호·세미콜론](1-3-statements-braces-semicolons.md)에서 C 문장과 함수 본문의 경계를 학습한다.

## 14. 참고 자료

- [N1570 6.10.2 Source file inclusion](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf#page=149): `#include` 지시문과 헤더 이름 형식. 공개 C11 초안이며 C17의 공개 참고 문서다.
- [GCC CPP: Header Files](https://gcc.gnu.org/onlinedocs/cpp/Header-Files.html): 헤더의 목적.
- [GCC CPP: Include Syntax](https://gcc.gnu.org/onlinedocs/cpp/Include-Syntax.html): GCC의 헤더 검색 규칙.
- [cppreference: main function](https://en.cppreference.com/w/c/language/main_function): 호스트 환경 `main`의 맥락.
