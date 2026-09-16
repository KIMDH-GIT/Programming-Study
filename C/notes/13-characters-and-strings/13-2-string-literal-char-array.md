# 13-2. 문자열 `"A"`와 문자 배열

문자 상수 `'A'`와 문자열 literal `"A"`는 type과 저장 element 수가 다르다.

## 1. 학습 목표
- character constant와 string literal을 구분한다.
- string literal의 null character 포함을 확인한다.
- char array의 element count와 표시 문자 수를 구분한다.

## 2. 선수 지식
Step 13-1의 character constant와 Part 11의 array initialization을 안다.

## 3. 핵심 개념
`'A'`는 type `int`인 character constant 하나다. `"A"`는 `A` 뒤에 null character `'\0'`을 포함하는 character array 형태의 string literal이다. `char text[] = "A";`는 두 elements를 가진 수정 가능한 array를 초기화한다.

## 4. 문법
```c
char letter = 'A';
char text[] = "A";
char same[] = {'A', '\0'};
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    char text[] = "A";

    printf("%s\n", text);
    printf("elements: %zu\n", sizeof(text) / sizeof(text[0]));
    printf("literal bytes: %zu\n", sizeof("A"));
    return 0;
}
```

## 6. 코드 해석
`text`는 `{'A', '\0'}`과 같은 두 `char` elements를 가진다. `%s`는 null character 전까지 `A`를 출력한다. `sizeof(text)`와 `sizeof("A")`는 모두 2 C bytes다.

## 7. 내부 동작
[C17 표준] ordinary string literal은 null character로 끝나는 character array다. 사람이 보는 내용 길이 1과 array element count 2는 다르다. 문자열 literal의 수정 규칙과 pointer 표현은 후속 Part에서 더 정확히 다룬다.

## 8. 자주 하는 실수
- `'A'`와 `"A"`를 같은 type이라고 생각한다.
- `"A"`가 `char` 하나만 차지한다고 생각한다.
- null terminator를 array 크기에서 빼먹는다.
- 문자열은 pointer라고 설명한다.

## 9. 필수 실습
`"Cat"`으로 char array를 초기화하고 `sizeof`로 element count를 확인한다. [실습 README](../../exercises/13-characters-and-strings/13-2/README.md)

## 10. 추가 실습
- ★ `"A"`와 `'A'`의 `sizeof` 비교
- ★★ explicit char initializer로 `"Cat"`과 같은 배열 만들기
- ★★★ 내용 길이와 array count 표 작성

## 11. 확인 문제
1. `'A'`의 type은?
2. `"A"`에는 몇 char elements가 있는가?
3. `char word[] = "Cat";`의 element count는?
4. 마지막 element 값은?
5. 모든 char array가 string인가?

## 12. 핵심 정리
character constant는 하나의 `int` 값이고 string literal은 마지막 `'\0'`까지 포함하는 character array다.

## 13. 다음 Step
[Step 13-3. 문자열의 `'\0'` 종료](13-3-null-termination.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.4.5, 6.7.9
- [cppreference: String literal](https://en.cppreference.com/w/c/language/string_literal.html)
