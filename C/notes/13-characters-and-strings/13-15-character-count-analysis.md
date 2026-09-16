# 13-15. 문자·대문자·소문자·숫자·공백 개수 분석

`<ctype.h>` classification functions를 사용하면 locale에 따른 character 분류를 C library 규칙으로 수행할 수 있다.

## 1. 학습 목표
- string을 terminator까지 순회하며 categories를 센다.
- `isupper`, `islower`, `isdigit`, `isspace`를 사용한다.
- ctype argument를 `unsigned char`로 안전하게 변환한다.

## 2. 선수 지식
Step 13-3의 string traversal과 Part 8의 condition chain을 사용한다.

## 3. 핵심 개념
ctype functions의 argument는 `EOF` 또는 `unsigned char`로 표현 가능한 값이어야 한다. plain `char`는 구현에 따라 signed일 수 있으므로 string element는 `(unsigned char)`로 변환해 전달한다. classification은 현재 locale의 영향을 받을 수 있으며 ASCII code range 직접 비교와 동일한 보장이 아니다.

## 4. 문법
```c
#include <ctype.h>
unsigned char value = (unsigned char)text[i];
if (isupper(value)) { ++uppercase_count; }
```

## 5. 최소 코드 예제
```c
#include <ctype.h>
#include <stdio.h>

int main(void)
{
    char text[] = "Az9 \t!";
    size_t uppercase_count = 0;
    size_t lowercase_count = 0;
    size_t digit_count = 0;
    size_t space_count = 0;

    for (size_t i = 0; text[i] != '\0'; ++i) {
        unsigned char value = (unsigned char)text[i];
        if (isupper(value)) {
            ++uppercase_count;
        } else if (islower(value)) {
            ++lowercase_count;
        } else if (isdigit(value)) {
            ++digit_count;
        } else if (isspace(value)) {
            ++space_count;
        }
    }

    printf("upper: %zu\n", uppercase_count);
    printf("lower: %zu\n", lowercase_count);
    printf("digit: %zu\n", digit_count);
    printf("space: %zu\n", space_count);
    return 0;
}
```

## 6. 코드 해석
기본 C locale에서 A는 uppercase, z는 lowercase, 9는 digit, space와 tab은 whitespace로 세어진다. `!`은 이 네 counters 중 어디에도 포함되지 않는다.

## 7. 내부 동작
[C17 표준] ctype argument 범위를 위반하면 behavior가 정의되지 않는다. `(unsigned char)` 변환이 음수 plain `char` 전달 위험을 막는다. [locale 구현] classification은 current locale의 영향을 받을 수 있다. UTF-8 multibyte sequence의 사람이 보는 한 글자를 char 하나로 분류한다고 일반화하지 않는다.

## 8. 자주 하는 실수
- plain `char`를 변환 없이 ctype function에 전달한다.
- `isspace`가 space character 하나만 찾는다고 생각한다.
- ASCII 숫자 범위를 C의 모든 locale 규칙으로 일반화한다.
- UTF-8 한글 한 글자를 char 하나로 센다.

## 9. 필수 실습
ASCII basic characters로 구성된 string에서 네 category counts를 출력한다. [실습 README](../../exercises/13-characters-and-strings/13-15/README.md)

## 10. 추가 실습
- ★ punctuation count 추가
- ★★ newline과 tab 포함
- ★★★ locale-dependent 분류와 byte sequence 차이 조사

## 11. 확인 문제
1. ctype argument의 유효 범위는?
2. `(unsigned char)` 변환을 하는 이유는?
3. plain `char` signedness는 항상 같은가?
4. `isspace`는 tab을 분류할 수 있는가?
5. UTF-8 문자 하나가 항상 char 하나인가?

## 12. 핵심 정리
valid ctype argument와 locale 구분을 지키며 null-terminated string의 각 byte character를 category별로 센다.

## 13. 다음 Step
[Step 13-16. Part 13 종합 복습](13-16-part-13-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.4
- [cppreference: Character classification](https://en.cppreference.com/w/c/string/byte.html#Character_classification)
