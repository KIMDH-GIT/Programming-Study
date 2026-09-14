# Step 1-1. 첫 프로그램과 `int main(void)`

기준은 C17의 호스트 환경(hosted environment)이다. 이 Step에서는 첫 C 프로그램의 시작 함수와 종료를 구분한다.

## 1. 학습 목표

- 호스트 환경에서 `main`이 프로그램 시작과 맺는 관계를 설명한다.
- `int main(void)`의 반환형, 이름, 매개변수 목록을 읽는다.
- `return 0;`의 정상 종료 의미를 안다.
- 최소 C17 프로그램을 빌드하고 실행 결과를 확인한다.

## 2. 선수 지식

터미널에서 `.c` 파일을 만들고 명령을 실행할 수 있으면 된다. [Part 0의 빌드 개요](../00-compilation/0-1-compilation-overview.md)를 읽었다면 소스, 빌드, 실행이 다른 작업임을 알고 있으면 충분하다.

## 3. 핵심 개념

함수는 이름이 붙은 실행 단위다. **[C 표준]** 호스트 환경에서 프로그램이 시작하면 C 추상 환경은 지정된 `main` 함수를 호출한다. OS가 C 소스의 `main`을 직접 호출한다고 표현하면 시작 코드와 런타임의 역할을 섞게 된다. 실제 실행 파일의 첫 기계 명령도 보통 `main`의 첫 문장이 아니다.

`int main(void)`에서 `int`는 반환형, `main`은 시작 함수 이름, `(void)`는 매개변수가 없음을 뜻한다. `{`와 `}` 사이가 함수 본문이다. 초기 `main` 호출에서 `return 0;`을 실행하면 정상 종료한다. 화면 출력과 종료 상태는 별개이며, **[GNU/Linux 구현]** 셸에서는 정상 종료가 보통 상태 0으로 보인다.

## 4. 문법

```c
int main(void)
{
    /* 문장 */
    return 0;
}
```

| 부분 | 의미 |
|---|---|
| `int` | 호출자에게 돌려주는 값의 형식 |
| `main` | 호스트 환경의 시작 함수 이름 |
| `(void)` | 매개변수가 없음을 명시 |
| `{ ... }` | 함수 본문 블록 |
| `return 0;` | 함수 실행을 끝내고 0을 반환 |

`void main(void)`은 일반적인 호스트 환경의 표준 형태가 아니다. C에서 `int main()`은 매개변수가 없다는 선언이 아니라 매개변수 형식을 지정하지 않은 선언이므로, 처음에는 `int main(void)`를 쓴다.

## 5. 최소 코드 예제

`first-program.c`에 저장한다.

```c
#include <stdio.h>

int main(void)
{
    puts("Hello, C!");
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -pedantic first-program.c -o first-program && ./first-program
```

## 6. 코드 해석

| 줄/토큰 | 해석 |
|---|---|
| `#include <stdio.h>` | `puts` 선언을 제공받는 전처리 지시문 |
| `int` | `main`의 반환형 |
| `main` | 호스트 환경의 시작 함수 이름 |
| `(void)` | 매개변수 없음 |
| `puts("Hello, C!");` | 문자열 한 줄을 표준 출력에 쓰는 함수 호출 |
| `return 0;` | 0을 반환해 정상 종료를 나타내는 문장 |

## 7. 내부 동작

1. **[C 표준]** C 추상 환경이 프로그램 시작 시 `main`을 호출한다.
2. **[GCC 구현]** 링크할 때 사용자가 쓴 오브젝트 외에 시작 코드와 C 라이브러리 구성 요소가 연결될 수 있다.
3. **[GNU/Linux 구현]** 셸의 실행 요청 뒤 Linux와 런타임이 프로세스 시작 상태를 준비하고 실행 흐름이 `main`에 도달한다.
4. **[C 표준]** 초기 `main`에서 반환하면 `exit` 호출에 대응하는 정상 종료 처리가 일어난다.

## 8. 자주 하는 실수

- OS가 C 함수 `main`을 직접 호출한다고 단정한다.
- `(void)`를 반환형으로 읽는다. 반환형은 `int`다.
- `int main()`이 C에서 매개변수 없음과 같다고 생각한다.
- `void main(void)`을 사용한다.
- `return 0;`이 화면에 0을 출력한다고 생각한다.
- 빌드 성공과 실행 성공을 같은 것으로 생각한다.

## 9. 필수 실습

### 시작 함수와 종료 상태 관찰하기

- **목적:** `main`의 형태, 출력, 종료 상태를 구별한다.
- **해야 할 일:** `exercises/01-program-structure/1-1/first-program.c`를 직접 만들고, 문자열을 바꿔 빌드·실행한다. 실행 직후 상태를 기록한다.
- **사용할 개념:** `int main(void)`, `puts`, `return 0;`, `$?`, 빌드와 실행.
- **예상되는 관찰 결과:** 선택한 문장 한 줄이 출력되고, **[GNU/Linux 구현]** 셸에서 보통 상태 0이다.
- **확인 포인트:** `int main(void)`가 정확히 한 번 있는가? 출력과 종료 상태를 따로 기록했는가?

명령과 기록 양식은 [1-1 실습 README](../../exercises/01-program-structure/1-1/README.md)에 있다.

## 10. 추가 실습

- ★ 문자열만 두 번 바꾸어 재빌드하고, 소스 변경과 실행 결과를 기록한다.
- ★★ `return 0;`을 뺀 `implicit-return.c`를 별도로 만들어 명시적 반환과 비교한다.
- ★★★ `gcc -S first-program.c -o first-program.s`로 만든 텍스트에서 `main` 기호를 찾는다.

## 11. 확인 문제

1. `int main(void)`의 반환형은 무엇인가?
2. `(void)`는 무엇을 뜻하는가?
3. `return 0;`은 무엇을 출력하는가?
4. 프로그램 시작 시 누가 `main`을 호출하는가?
5. 빌드만 한 뒤 출력이 보이지 않는 이유는 무엇인가?

<details>
<summary>정답과 해설</summary>

1. `int`다.
2. 매개변수가 없음을 뜻한다.
3. 아무것도 출력하지 않고 0을 반환한다.
4. 호스트 환경의 C 추상 환경이다.
5. 빌드와 실행은 별도 작업이므로 `./first-program`으로 실행해야 한다.

</details>

## 12. 핵심 정리

호스트 환경의 프로그램은 C 추상 환경이 호출하는 `main`에서 시작한다. `int main(void)`은 정수를 반환하고 매개변수를 받지 않으며, `return 0;`은 정상 종료를 나타낸다.

## 13. 다음 Step

[Step 1-2. `#include`와 표준 헤더](1-2-include-standard-headers.md)에서 `puts` 선언과 전처리 지시문을 다룬다.

## 14. 참고 자료

- [N1570 5.1.2.2.1 Program startup](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf#page=182): `main` 형식과 시작·종료. 공개 C11 초안이며 C17의 공개 참고 문서다.
- [cppreference: main function](https://en.cppreference.com/w/c/language/main_function): `main`의 허용 형태와 종료 의미.
- [GCC Overall Options](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html): `-S`, `-o` 등 GCC 옵션.
