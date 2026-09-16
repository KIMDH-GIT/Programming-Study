# 5-4. `%c`, `%s`

`%c`는 문자 하나, `%s`는 null 문자로 끝나는 문자열을 출력한다. 둘은 비슷해 보여도 요구하는 인자와 읽는 범위가 다르다.

## 1. 학습 목표

- `%c`와 `%s`의 인자와 출력 단위를 구별한다.
- 문자열 끝의 `'\0'` 필요성을 설명한다.
- `%s` 정밀도로 최대 출력 문자 수를 제한한다.

## 2. 선수 지식

문자 상수와 문자열 리터럴을 사용한다. 문자 배열의 자세한 구조는 Part 13에서 배운다.

## 3. 핵심 개념

`%c`는 기본 인수 승격을 거친 `int` 값을 `unsigned char`로 변환한 문자를 쓴다. `%s`는 문자 sequence를 `'\0'`까지 읽는다. 유효한 종료 문자가 없으면 객체 범위를 넘어 읽어 Undefined Behavior가 될 수 있다. `%.5s`는 최대 5문자만 출력한다.

## 4. 문법

```c
printf("%c %s %.3s\n", 'A', "Hello", "World");
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    char letter = 'C';

    printf("letter=%c\n", letter);
    printf("word=%s\n", "language");
    printf("prefix=%.4s\n", "language");
    return 0;
}
```

## 6. 코드 해석

1. `letter`는 승격되어 `%c`에 전달된다.
2. 문자열 리터럴은 끝에 null 문자를 가진다.
3. `%.4s`는 앞의 네 문자만 출력한다.
4. 정밀도는 원본 문자열을 수정하지 않는다.

## 7. 내부 동작

`printf`는 `%s` 인자가 가리키는 첫 문자부터 종료 null 문자 또는 정밀도 한계까지 읽는다. 문자 인코딩과 다중 byte 문자의 표시 결과는 실행 환경에 따라 달라질 수 있다.

## 8. 자주 하는 실수

- 문자 하나에 `%s`를 사용한다.
- 문자열에 `%c`를 사용한다.
- null 종료가 없는 저장 공간을 `%s`에 넘긴다.
- `%s` 정밀도를 입력 버퍼 보호 기능으로 오해한다.

## 9. 필수 실습

문자 하나와 문자열 전체·접두 부분을 출력한다. [실습 README](../../exercises/05-input-output/5-4/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 서로 다른 문자 세 개를 출력한다.
- ★★ **응용:** 문자열 정밀도 2, 5를 비교한다.
- ★★★ **도전:** null 종료가 필요한 이유를 범위 관점에서 설명한다.

## 11. 확인 문제

1. `%c`와 `%s`는 각각 무엇을 출력하는가?
2. `%s`는 어디에서 읽기를 멈추는가?
3. `%.3s`는 원본을 바꾸는가?
4. null 종료가 없으면 어떤 위험이 있는가?

## 12. 핵심 정리

`%c`는 문자 하나, `%s`는 null 종료 문자열이다. `%s` 정밀도는 최대 출력 문자 수를 제한하지만 유효한 인자 요구를 없애지 않는다.

## 13. 다음 Step

[Step 5-5. `%p`, `(void *)`와 주소 출력 예고](5-5-pointer-output-preview.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.1.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
- [cppreference: string literal](https://en.cppreference.com/w/c/language/string_literal.html)
