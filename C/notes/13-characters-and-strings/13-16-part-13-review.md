# 13-16. Part 13 종합 복습

Part 13에서는 `char`, character constants, null-terminated strings, bounded input, 직접 구현과 string library의 contract를 학습했다.

## 1. 학습 목표
- character value·char array·valid string을 구분한다.
- null terminator, length, capacity 규칙을 종합한다.
- C17 보장과 encoding·locale 구현을 구분한다.

## 2. 선수 지식
Step 13-1부터 13-15까지와 Part 5·11의 I/O와 arrays를 사용한다.

## 3. 핵심 개념
`char`는 integer type이고 ordinary character constant는 `int`다. C string은 접근 가능한 `'\0'`으로 끝나는 character sequence다. `sizeof`는 array storage, `strlen`은 terminator 전 length다. copy와 concatenation은 destination capacity가 충분해야 하며 comparison result는 sign으로 해석한다.

## 4. 문법
```c
char text[] = "C17";
size_t length = strlen(text);
size_t capacity = sizeof(text);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char text[] = "C17";

    printf("%s\n", text);
    printf("length: %zu\n", strlen(text));
    printf("array bytes: %zu\n", sizeof(text));
    printf("digit value: %d\n", text[2] - '0');
    return 0;
}
```

## 6. 코드 해석
text는 C, 1, 7, terminator의 four elements다. `%s`는 terminator 전까지 출력하고 `strlen`은 3, `sizeof`는 4다. C가 digit characters의 연속성을 보장하므로 `'7' - '0'`은 7이다.

## 7. 내부 동작
[C17 표준] valid string contract, array bounds, library destination capacity, ctype argument 범위를 지켜야 한다. null 없는 `%s`, buffer overflow, literal 수정은 실행해서 관찰할 실습이 아니다. [구현 관점] plain `char` signedness, character encoding, locale classification은 구현에 따라 달라질 수 있다. 문자열이나 배열을 pointer와 동일시하지 않는다.

## 8. 자주 하는 실수
- `'A'`의 type을 `char`라고 말한다.
- C가 ASCII code values를 강제한다고 생각한다.
- `'0'`과 `'\0'`을 혼동한다.
- 모든 char array를 valid string처럼 `%s`에 사용한다.
- copy·concatenation capacity에서 terminator 자리를 빼먹는다.

## 9. 필수 실습
한 char array에서 `%s`, `strlen`, `sizeof`, digit conversion을 함께 검증한다. [실습 README](../../exercises/13-characters-and-strings/13-16/README.md)

## 10. 추가 실습
- ★ 직접 length와 library length 비교
- ★★ safe copy 후 comparison
- ★★★ C17·encoding·locale·pointer 후속 범위 점검표

## 11. 확인 문제
1. ordinary character constant의 type은?
2. string terminator 값은?
3. char array가 valid string이 되려면?
4. `sizeof("Cat")`과 `strlen("Cat")`은?
5. `strcmp` result에서 보장되는 것은?
6. ctype에 plain char를 그대로 넘길 때의 함정은?
7. C가 UTF-8 또는 ASCII를 모든 구현에 강제하는가?

## 12. 핵심 정리
문자열 코드는 terminator·length·capacity·argument contracts를 지키고 C 표준과 문자 encoding 구현을 분리해야 안전하다.

## 13. 다음 Step
Step 14-1. 메모리 주소란 무엇인가

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.4.4.4, 6.4.5, 7.4, 7.21, 7.24
- [cppreference: Null-terminated byte strings](https://en.cppreference.com/w/c/string/byte.html)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
