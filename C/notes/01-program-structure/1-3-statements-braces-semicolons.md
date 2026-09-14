# Step 1-3. statement, 중괄호, 세미콜론

기준은 C17 호스트 환경이다. 이 Step에서는 실행의 기본 문법 단위인 문장(statement)과 중괄호로 만든 블록을 읽는다.

## 1. 학습 목표

- statement, compound statement(블록), 함수 본문, null statement를 구별한다.
- 세미콜론이 필요한 위치와 필요하지 않은 위치를 문법으로 설명한다.
- 짧은 C17 프로그램에서 문장과 중괄호의 짝을 정확히 찾는다.

## 2. 선수 지식

- [Step 1-1](1-1-first-program-main.md)의 `int main(void)` 함수 정의.
- [Step 1-2](1-2-include-standard-headers.md)의 전처리 지시문과 C 문장의 구분.

## 3. 핵심 개념

**[C 표준]** N1570 6.8에서 statement는 실행의 문법 단위다. expression statement, compound statement, selection statement, iteration statement, jump statement, labeled statement가 있다. 그러므로 모든 문장이 “식 + 세미콜론”인 것은 아니다.

`{`와 `}` 사이의 **복합 문장(compound statement)**, 즉 **블록**은 statement 하나다. 블록 안에는 statement뿐 아니라 선언(declaration)도 block item으로 올 수 있다. `int main(void) { ... }`의 `{ ... }`는 함수 정의의 **함수 본문**이며 복합 문장이다. 함수 정의를 닫는 `}` 뒤에는 세미콜론을 붙이지 않는다.

**[C 표준]** `;` 하나만으로 이루어진 **널 문장(null statement)**도 있다. 이는 식이 비어 있는 expression statement다. `printf("Hi\n");`는 함수 호출 식과 끝의 `;`로 이루어진 expression statement다. `return 0;`은 jump statement이고, 블록과 `if` 같은 제어문은 고유한 문법을 가진다.

## 4. 문법

```text
expression-statement: expression(opt) ;
compound-statement: { block-item-list(opt) }
block-item: declaration | statement
```

`return 0;`은 expression statement가 아니라 `return` jump statement다. `;` 하나는 빈 식을 가진 expression statement다. 반대로 `int work(void);`는 함수 **선언**을 끝내는 세미콜론이고, `int work(void) { return 0; }`는 함수 **정의**이므로 마지막 `}` 뒤에 세미콜론이 없다.

```c
int main(void)
{
    ;
    return 0;
}
```

## 5. 최소 코드 예제

```c
int main(void)
{
    ;
    return 0;
}
```

출력 없이 문장 경계만 관찰하는 최소 C17 예제다. `main`이 `int`를 반환하므로 `return 0;`은 호출 환경에 성공 종료 값을 돌려준다.

## 6. 코드 해석

| 줄 | 토큰·구성 | 해석 |
|---|---|---|
| 1 | `int main ( void )` | 함수 정의의 머리다. 끝에 `;`이 없다. |
| 2 | `{` | 함수 본문인 복합 문장을 시작한다. |
| 3 | `;` | 식이 비어 있는 expression statement, 즉 널 문장이다. |
| 4 | `return 0 ;` | `return` jump statement다. `0`은 반환식이고 `;`가 문장을 끝낸다. |
| 5 | `}` | 복합 문장과 함수 본문을 닫는다. 뒤에 세미콜론은 없다. |

공백과 줄바꿈은 대부분 토큰을 구분할 뿐 문장 자체가 아니다. 줄 3의 `;`와 줄 4 끝의 `;`는 서로 다른 문장을 만든다.

## 7. 내부 동작

**[C 표준]** 번역 중 토큰은 문법에 맞춰 분석된다. `{` 뒤에서는 block item 목록을 읽고, 대응하는 `}`에서 복합 문장이 끝난다. 중괄호는 실행 중에 열고 닫는 명령이 아니라 소스 구조를 나타낸다.

**[GCC 구현]** 컴파일러는 이 구조를 내부 표현으로 만들어 `return 0;`의 제어 흐름을 생성한다. 널 문장은 관찰 가능한 동작이 없으므로 보통 기계 명령을 만들 필요가 없다. 실제 어셈블리 모양은 GCC 버전, 최적화, 대상 CPU에 따라 달라진다.

## 8. 자주 하는 실수

- **함수 정의의 `}` 뒤에 `;`을 붙인다.** 함수 정의는 본문을 닫는 `}`로 끝난다.
- **모든 `}` 뒤에 `;`을 붙인다.** 블록과 제어문에는 해당하지 않는다. 구조체 선언처럼 다른 문법에는 필요할 수 있으므로 문맥을 읽는다.
- **모든 statement를 식과 `;`로 본다.** 블록, 제어문, `return`은 그 형태가 아니다.
- **빈 줄을 널 문장으로 본다.** 빈 줄에는 토큰이 없고 널 문장에는 실제 `;` 토큰이 있다.
- **중괄호 하나를 빼고 다음 줄만 고친다.** 진단 위치가 뒤 토큰일 수 있으므로 먼저 `{`와 `}`의 짝을 확인한다.

## 9. 필수 실습

### 실습 A. 문장 경계 표시하기

- **목적:** 세미콜론과 중괄호를 보고 문장 경계를 판단한다.
- **해야 할 일:** [실습 안내](../../exercises/01-program-structure/1-3/README.md)에 따라 `statements.c`를 작성한다. `main` 본문에 널 문장 하나와 `return 0;` 하나를 넣고, 각 토큰의 역할을 별도 기록에 표시한다.
- **사용할 개념:** 함수 정의, 복합 문장, 널 문장, jump statement.
- **예상 관찰 결과:** `gcc -std=c17 -Wall -Wextra -pedantic statements.c -o statements && ./statements`가 성공한다. 출력은 없다.
- **확인 포인트:** 함수 본문 `}` 뒤에 `;`이 없는가? `;` 하나가 널 문장임을 설명할 수 있는가?

## 10. 추가 실습

- ★ 널 문장을 지우고 다시 빌드한다. 실행 결과와 소스 구조에서 달라진 점을 기록한다.
- ★★ `return 0;`의 세미콜론 하나만 지운 복사본을 빌드한다. GCC가 가리킨 행과 실제로 빠진 토큰을 따로 기록한다.

## 11. 확인 문제

1. `{ ... }`는 C 문법에서 무엇인가?
2. 함수 본문의 닫는 `}` 뒤에 세미콜론이 필요한가?
3. `;` 하나는 빈 줄과 같은가?
4. `return 0;`는 expression statement인가?
5. 블록 안에는 statement만 올 수 있는가?

<details>
<summary>정답과 해설 펼치기</summary>

1. 복합 문장(compound statement), 즉 블록이다.
2. 아니다. 함수 정의는 본문을 닫는 `}`로 끝난다.
3. 아니다. `;`는 널 문장이라는 토큰이고 빈 줄에는 토큰이 없다.
4. 아니다. `return`은 jump statement다.
5. 아니다. 선언과 statement가 모두 block item으로 올 수 있다.

</details>

## 12. 핵심 정리

- statement는 넓은 문법 범주이며 모두 식과 세미콜론의 조합은 아니다.
- `{ ... }`는 하나의 복합 문장이고 함수 본문으로도 사용된다.
- `;` 하나는 널 문장이다.
- 세미콜론 필요 여부는 앞의 문법이 결정한다. 닫는 중괄호마다 붙이지 않는다.

## 13. 다음 Step

[Step 1-4. `printf`와 여러 줄 출력](1-4-printf-multiline-output.md)에서 함수 호출 문장으로 표준 출력에 문자열을 쓴다.

## 14. 참고 자료

- [N1570 6.8 Statements and blocks](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): statement와 block 문법. N1570은 C11 공개 초안이며 C17 기준 학습의 공개 참고 자료다.
- [N1570 6.8.2 Compound statement](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 복합 문장과 block item.
- [cppreference: Statements](https://en.cppreference.com/w/c/language/statements): C statement 종류의 개요.
