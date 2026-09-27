# 27-11. compile 성공과 프로그램 정확성의 차이
## 1. 학습 목표
- translation success, diagnostic, runtime behavior, correctness를 구분한다.
- compiler가 반드시 진단하지 않는 UB 사례를 안다.
- library function의 argument contract를 검사한다.
## 2. 선수 지식
Part 0의 compile·link 단계, Part 5의 format, Part 13·18·24·25의 library·link·macro를 안다.
## 3. 핵심 개념
`gcc -std=c17 ...` 성공은 executable이 만들어졌다는 뜻이지 모든 execution이 UB-free라는 증명이 아니다. C17은 syntax rule 또는 constraint violation에 적어도 하나의 diagnostic을 요구하지만, 다른 UB에는 diagnostic이 요구되지 않을 수 있다. implementation은 diagnostic 후에도 번역을 계속할 수 있다.

compile error와 linker error는 실행 중 UB가 아니다. 특히 linker의 `undefined reference`에서 “undefined”는 Undefined Behavior라는 기술 용어와 무관하다.
## 4. 문법
library 호출도 precondition을 지켜야 한다.

⚠ 분석용 — 실행하지 않는다.
```c
unsigned int x = 10;
printf("%d\n", x);       /* conversion specification과 argument type 불일치 */

double value;
scanf("%d", &value);     /* destination type 불일치 */

char *text = "hello";
text[0] = 'H';           /* string literal 수정: UB */

int bad = EOF == -1 ? -2 : -1;
isalpha(bad);            /* 항상 음수이고 EOF가 아니므로 ctype domain 위반 */
```
문자열 리터럴이 특정 OS의 `.rodata`에 놓이기 때문이 아니라, C17이 literal로 만들어진 배열의 수정을 UB로 규정하기 때문에 수정할 수 없다. `char text[] = "hello";`는 별개의 수정 가능한 배열이다.

겹치는 object 사이 복사는 `memmove`를 사용한다. overlap이 있는 `memcpy`는 implementation-defined가 아니라 contract 위반으로 UB다.
## 5. 최소 코드 예제
```c
#include <ctype.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    char text[] = "hello";
    unsigned char ch = (unsigned char)text[0];

    text[0] = (char)toupper(ch);
    memmove(text + 1, text, 4);
    printf("%s\n", text);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o correctness
./correctness
```
## 6. 코드 해석
수정 가능한 배열을 사용하고, `toupper`에는 `unsigned char`로 표현 가능한 값을 전달하며, 겹치는 범위는 `memmove`로 복사한다. 출력은 `HHell`이다.
## 7. 내부 동작
**[C17]** variadic format argument, `ctype` domain, string literal modification, library pointer·buffer·overlap 조건은 각각 표준 contract를 따른다. 위반 시 해당 규칙이 정한 UB가 될 수 있다.

**[compiler diagnostic]** format warning 등은 유용하지만 option·data flow·호출 가시성에 따라 모든 문제를 검출하지 않는다.

**[linker]** missing definition이나 multiple definition report는 translation/link 단계 문제이며 runtime UB 정의가 아니다.

**[runtime / OS]** 원하는 출력과 정상 exit도 모든 precondition 준수를 증명하지 않는다.
## 8. 자주 하는 실수
- `-Wall -Wextra -Wpedantic -Werror` 통과를 UB-free 증명으로 쓴다.
- syntax error·compile error·link error를 UB라고 부른다.
- string literal은 `.rodata`라서만 수정할 수 없다고 한다.
- negative plain `char`를 그대로 `ctype` 함수에 넘긴다.
- overlap `memcpy` 결과를 implementation-defined라고 한다.
## 9. 필수 실습
modifiable array, correct format, ctype domain, `memmove`를 사용한 정상 program을 작성한다.
[27-11 exercise](../../exercises/27-undefined-behavior/27-11/README.md)
## 10. 추가 실습
- ★ `%p`로 object pointer를 `(void *)` cast해 출력한다.
- ★★ `printf`와 `scanf`의 type 대응표를 만든다.
- ★★★ library precondition checklist를 작성한다.
## 11. 확인 문제
1. compile 성공이 보장하는 것과 보장하지 않는 것은?
2. constraint violation에는 어떤 요구가 있는가?
3. `undefined reference`가 UB가 아닌 이유는?
4. `ctype` 함수의 argument domain은?
5. overlap copy에서 `memmove`가 필요한 이유는?
6. string literal과 character array의 차이는?
## 12. 핵심 정리
- translation success, diagnostics, runtime observation, correctness는 서로 다르다.
- library function도 type·range·pointer·buffer precondition을 지킨다.
- strict warnings는 필수 도구지만 완전한 UB 검출기는 아니다.
## 13. 다음 Step
[27-12. Part 27 종합 복습](27-12-part-27-review.md)
## 14. 참고 자료
- N1570 5.1.1.3, 6.4.5p7, 7.1.1, 7.4p1, 7.21.6.1, 7.21.6.2, 7.24.2.1-2, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf)
- [cppreference: byte strings](https://en.cppreference.com/w/c/string/byte)
