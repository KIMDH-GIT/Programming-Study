# 13-1. `char`와 문자 `'A'`

`char`는 C의 integer type 중 하나이며 문자 데이터를 저장할 때 흔히 사용한다.

## 1. 학습 목표
- `char`가 정수형임을 설명한다.
- ordinary character constant `'A'`의 type이 `int`임을 구분한다.
- 문자 표현과 정수값 관찰을 C 표준과 encoding 구현으로 나눈다.

## 2. 선수 지식
Part 2의 integer types, Part 5의 `%c`·`%d`, Part 6의 conversion을 사용한다.

## 3. 핵심 개념
`char c = 'A';`에서 `char`는 객체 type이고 `'A'`는 ordinary character constant다. C17에서 ordinary character constant의 type은 `int`다. 그 값이 `char` 객체를 초기화한다. `char`는 문자 전용으로 정수와 분리된 type이 아니며 arithmetic에 참여할 때 integer promotion이 적용될 수 있다.

## 4. 문법
```c
char letter = 'A';
printf("%c\n", letter);
printf("%d\n", (int)letter);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    char letter = 'A';
    char digit = '7';

    printf("%c\n", letter);
    printf("%d\n", (int)letter);
    printf("%d\n", digit - '0');
    return 0;
}
```

## 6. 코드 해석
첫 줄은 문자 `A`를 표시한다. 둘째 줄은 현재 실행 문자 집합에서 저장된 정수값을 보여 준다. 셋째 줄은 C가 `'0'`부터 `'9'`까지 연속된 값을 보장하므로 숫자 문자 `7`을 정수 7로 바꾼다.

## 7. 내부 동작
[C17 표준] `'A'`의 type은 `int`이고 `%c` argument에는 default argument promotions가 적용된 `int` 값이 전달된다. [문자 encoding 구현] C가 모든 구현에 ASCII 코드값을 강제하지 않으므로 `'A'`가 반드시 65라고 말할 수 없다. ASCII 계열 환경에서는 흔히 65로 관찰된다.

## 8. 자주 하는 실수
- `'A'`의 type을 `char`라고 단정한다.
- `char`를 정수형과 무관한 문자 전용 type이라고 설명한다.
- C 표준이 ASCII를 강제한다고 생각한다.
- 숫자 문자 `'7'`과 정수 7을 같은 값이라고 생각한다.

## 9. 필수 실습
문자 상수를 `char` 객체에 저장하고 `%c`와 `%d`로 두 관점을 출력한다. [실습 README](../../exercises/13-characters-and-strings/13-1/README.md)

## 10. 추가 실습
- ★ 숫자 문자에서 정수값 구하기
- ★★ `sizeof('A')`와 `sizeof(char)` 비교
- ★★★ C17 보장과 ASCII 관찰을 표로 구분

## 11. 확인 문제
1. `char`는 어떤 type 분류에 속하는가?
2. C17에서 `'A'`의 type은?
3. `%c`가 받는 promoted argument type은?
4. C가 `'A' == 65`를 모든 구현에 보장하는가?
5. `digit - '0'`이 가능한 표준상 이유는?

## 12. 핵심 정리
`char`는 integer type이고 ordinary character constant는 `int`이며, 문자 표시와 encoding의 정수값을 구분한다.

## 13. 다음 Step
[Step 13-2. 문자열 `"A"`와 문자 배열](13-2-string-literal-char-array.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 5.2.1, 6.4.4.4, 6.5.2.2
- [cppreference: Character constants](https://en.cppreference.com/w/c/language/character_constant.html)
- [cppreference: Character type](https://en.cppreference.com/w/c/language/arithmetic_types.html#Character_types)
