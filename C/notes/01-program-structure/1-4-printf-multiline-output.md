# Step 1-4. `printf`와 여러 줄 출력

기준은 C17 호스트 환경이다. 이 Step에서는 `printf`로 보통 문자열을 표준 출력에 쓰고 줄바꿈으로 여러 줄을 만든다. 값 서식 지정은 Part 5에서 다룬다.

## 1. 학습 목표

- `<stdio.h>`의 `printf` 선언을 포함하여 문자열을 출력한다.
- `\n`이 출력 줄을 끝내는 escape sequence임을 확인한다.
- `printf` 호출 문장, 표준 출력, 반환값을 구별한다.

## 2. 선수 지식

- [Step 1-2](1-2-include-standard-headers.md)의 표준 헤더 포함.
- [Step 1-3](1-3-statements-braces-semicolons.md)의 함수 호출 뒤 세미콜론과 함수 본문 블록.

## 3. 핵심 개념

**[C 표준]** `printf`는 `<stdio.h>`에 선언된 함수다. 첫 번째 인수인 format 문자열을 해석해 **stdout**(표준 출력 스트림)에 쓴다. 이 Step에서는 변환 지정자 없이 평범한 문자열과 줄바꿈만 사용한다.

문자열 리터럴 안의 `\n`은 소스에 `\`와 `n`을 그대로 출력하라는 뜻이 아니라 new-line character를 나타내는 escape sequence다. `printf("first\nsecond\n");`는 두 개의 new-line character를 출력하며, 일반적인 터미널에서는 보통 두 줄로 보인다.

**[C 표준]** 성공하면 `printf`는 stdout 스트림으로 전송한 문자 수를 반환하고, 출력 또는 인코딩 오류가 나면 음수를 반환한다. 여기서 전송은 스트림에 대한 결과이며, 문자가 물리 장치에 즉시 표시되거나 flush됐다는 뜻은 아니다. 이 반환값은 화면에 보이는 문자열이 아니며 여기서는 검사하지 않는다.

## 4. 문법

```c
#include <stdio.h>

int main(void)
{
    printf("first\nsecond\n");
    return 0;
}
```

`printf(...)`는 함수 호출 식이고, 뒤의 `;`까지가 expression statement다. 문자열 마지막 `\n`은 마지막 줄 뒤의 줄바꿈도 요청한다. 이 Step에서는 `%d`, `%s` 같은 변환 지정자, 추가 인수, 출력 폭·정밀도를 쓰지 않는다. 이는 Part 5의 서식 문자열과 인자 대응 주제다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    printf("Hello\nStart C17\n");
    return 0;
}
```

일반적인 터미널에서는 보통 다음 두 줄로 보인다. C 표준이 특정 화면 표시 방식을 요구하는 것은 아니다.

```text
Hello
Start C17
```

## 6. 코드 해석

| 줄 | 토큰·구성 | 해석 |
|---|---|---|
| 1 | `# include < stdio.h >` | 전처리 지시문이다. `printf` 선언을 제공한다. 끝에 `;`을 붙이지 않는다. |
| 2 | 빈 줄 | 읽기 위한 공백이며 실행 문장이 아니다. |
| 3 | `int main ( void )` | 프로그램 진입 함수 정의의 머리다. |
| 4 | `{` | `main`의 복합 문장을 시작한다. |
| 5 | `printf ( "Hello\nStart C17\n" ) ;` | 함수 호출 식과 이를 끝내는 세미콜론이다. 두 `\n`은 각각 new-line character 하나다. |
| 6 | `return 0 ;` | `main`에서 성공 종료 값을 반환하는 jump statement다. |
| 7 | `}` | 함수 본문을 닫는다. 뒤에 세미콜론은 없다. |

## 7. 내부 동작

**[C 표준]** 번역 중 문자열 리터럴의 escape sequence가 해당 문자로 구성된다. 실행 때 `printf`는 format 문자열을 읽어 stdout 스트림에 문자를 쓴다. 이 예제에는 변환 지정자가 없으므로 추가 값을 꺼내거나 숫자를 문자열로 바꾸지 않는다.

**[GNU/Linux 구현]** 터미널에서 실행하면 stdout은 보통 터미널에 연결되어 문자가 보인다. `./hello > output.txt`처럼 셸이 stdout을 파일로 재지정하면 같은 출력은 파일로 간다. 이는 `printf`가 화면 전용 함수가 아니라 stdout에 쓰는 함수임을 보인다. 셸 재지정은 C 문법이 아니다.

## 8. 자주 하는 실수

- **`#include <stdio.h>;`로 쓴다.** 전처리 지시문 끝에는 세미콜론을 붙이지 않는다.
- **`printf` 호출 끝의 `;`을 뺀다.** 함수 호출 식을 문장으로 끝내는 세미콜론이 필요하다.
- **문자열 리터럴 밖에 `\n`을 쓴다.** escape sequence는 문자열 또는 문자 상수 안에서 쓴다.
- **`\n`을 출력되는 두 글자라고 생각한다.** 소스 표기이며 출력에서는 줄바꿈 문자 하나다.
- **반환값을 성공 종료 상태로 생각한다.** `printf` 반환값은 문자 수 또는 오류이고 `main`의 `return 0;`과 다르다.
- **지금 `%d`를 익혀야 한다고 생각한다.** 서식 지정자는 Part 5에서 배운다.

## 9. 필수 실습

### 실습 A. 두 줄 자기소개 출력

- **목적:** 한 `printf` 호출의 문자열 안에서 줄바꿈을 사용한다.
- **해야 할 일:** [실습 안내](../../exercises/01-program-structure/1-4/README.md)에 따라 `multiline.c`를 작성한다. `<stdio.h>`를 포함하고, 한 번의 `printf` 호출로 본인이 정한 두 줄을 출력하며 각 줄 끝에 `\n`을 넣는다.
- **사용할 개념:** 표준 헤더, 함수 호출 expression statement, 문자열 리터럴, newline character, stdout.
- **예상 관찰 결과:** GCC 빌드가 성공하고 일반적인 터미널에서는 두 줄로 보인다. 마지막 문자열에도 new-line character가 있다.
- **확인 포인트:** `printf` 선언을 포함했는가? 호출 뒤에 세미콜론이 있는가? `%` 변환 지정자나 추가 인수를 쓰지 않았는가?

## 10. 추가 실습

- ★ 같은 두 줄을 `printf` 두 번으로 출력하는 `two_calls.c`를 작성한다. 출력 내용은 같게 하고 호출 문장 수만 비교한다.
- ★★ `./multiline > captured.txt`로 stdout을 파일에 재지정한 뒤 내용을 확인한다. 소스는 바꾸지 말고 stdout의 목적지만 셸이 바꾼다는 점을 기록한다.

## 11. 확인 문제

1. `printf`의 선언은 어느 표준 헤더에 있는가?
2. `printf`는 기본적으로 어느 스트림에 쓰는가?
3. 문자열 안의 `\n`은 무엇을 나타내는가?
4. `printf("Hi\n");`의 세미콜론은 무엇을 끝내는가?
5. 성공한 `printf`의 반환값은 무엇을 뜻하는가?
6. 이 Step에서 `%d` 같은 변환 지정자를 배우는가?

<details>
<summary>정답과 해설 펼치기</summary>

1. `<stdio.h>`다.
2. stdout, 즉 표준 출력 스트림이다.
3. newline character 하나다.
4. 함수 호출 식으로 만든 expression statement를 끝낸다.
5. stdout 스트림으로 전송한 문자 수다. 물리 장치에 즉시 표시됐다는 뜻은 아니며, 오류면 음수를 반환한다.
6. 아니다. 일반 문자열 출력과 줄바꿈만 다루며 서식 지정자는 Part 5에서 다룬다.

</details>

## 12. 핵심 정리

- `printf`는 `<stdio.h>`에 선언되고 stdout에 쓴다.
- `\n`은 문자열 안에서 줄바꿈 문자 하나를 나타낸다.
- 호출 뒤 `;`은 expression statement를 끝내며 함수 본문의 `}` 뒤에는 붙지 않는다.
- `printf`는 성공 시 문자 수, 오류 시 음수를 반환한다.

## 13. 다음 Step

[Step 1-5. `\n`, `\t`, `\"`, `\\` escape sequence](1-5-escape-sequences.md)에서 문자열 안의 다른 escape sequence를 다룬다.

## 14. 참고 자료

- [N1570 7.21.6.3 `printf`](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): stdout, 반환값, format 문자열. N1570은 C11 공개 초안이며 C17 기준 학습의 공개 참고 자료다.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf): `printf`의 선언과 반환값.
- [cppreference: Escape sequences](https://en.cppreference.com/w/c/language/escape): `\n`을 포함한 escape sequence 개요.
