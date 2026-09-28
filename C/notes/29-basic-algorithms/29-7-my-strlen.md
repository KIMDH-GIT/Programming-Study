# 29-7. `my_strlen`
## 1. 학습 목표
- C string의 null-termination precondition을 설명한다.
- terminating null character 전까지의 길이를 계산한다.
- empty string과 one-character string 경계를 검증한다.
## 2. 선수 지식
Part 13의 C string, Part 15의 array·pointer 관계, 29-1의 contract를 안다.
## 3. 핵심 개념
**[Algorithm]** 첫 character부터 `'\0'`을 만날 때까지 count를 증가시킨다.

입력 조건:

```text
text는 접근 가능한 null-terminated character sequence를 가리킨다.
```

단순한 char buffer가 자동으로 C string이 되는 것은 아니다. 접근 가능한 범위에 `'\0'`이 없으면 이 함수의 precondition 위반이다.
## 4. 문법
```c
static size_t my_strlen(const char text[])
{
    size_t length = 0;

    while (text[length] != '\0') {
        ++length;
    }
    return length;
}
```
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static size_t my_strlen(const char text[])
{
    size_t length = 0;

    while (text[length] != '\0') {
        ++length;
    }
    return length;
}

int main(void)
{
    printf("%zu\n", my_strlen("C17!!"));
    printf("%zu\n", my_strlen(""));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strlen
./my_strlen
```
## 6. 코드 해석
`"C17!!"`에서 다섯 non-null character를 센다. empty string은 첫 character가 이미 `'\0'`이므로 loop를 실행하지 않고 0을 반환한다.
## 7. 내부 동작
**[Algorithm]** invariant는 `[0, length)`에 null character가 없다는 것이다. 입력 length를 `n`이라 하면 time은 `O(n)`, auxiliary space는 `O(1)`이다.

**[C17]** string literal은 terminating null character를 포함한다. 함수는 precondition을 만족하는 범위만 읽으며 source를 수정하지 않으므로 `const char[]`를 받는다.

**[Compiler / CPU]** source character 검사 횟수와 machine instruction 수는 같지 않을 수 있다.
## 8. 자주 하는 실수
- null-terminated string과 임의 char buffer를 동일시한다.
- terminator 자체를 length에 포함한다.
- missing terminator buffer를 실행해 확인하려 한다.
- `int` length로 모든 object size를 표현한다고 일반화한다.
## 9. 필수 실습
empty, one-character, 일반 ASCII string의 expected length를 먼저 적고 검증한다.
[29-7 exercise](../../exercises/29-basic-algorithms/29-7/README.md)
## 10. 추가 실습
- ★ space를 포함한 string을 검사한다.
- ★★ standard `strlen` 결과와 임시 validation에서 비교한다.
- ★★★ pointer notation으로 다시 쓰고 readability를 비교한다.
## 11. 확인 문제
1. input precondition은 무엇인가?
2. empty string의 length가 0인 이유는?
3. terminator를 결과에 포함하지 않는 이유는?
4. function이 missing terminator를 안전하게 탐지할 수 없는 이유는?
5. 이 algorithm의 `n`은 무엇인가?
## 12. 핵심 정리
- C string은 null-terminated sequence라는 contract를 가진다.
- terminator 전 character 수만 반환한다.
- invalid buffer를 실행해 탐색하지 말고 caller가 precondition을 보장한다.
## 13. 다음 Step
[29-8. `my_strcpy`](29-8-my-strcpy.md)
## 14. 참고 자료
- N1570 7.1.1, 7.24.6.3. N1570은 **C11 공개 Committee Draft**이며 string·`strlen` 관련 규칙은 C17에서도 유지된다.
- [cppreference: null-terminated byte strings](https://en.cppreference.com/w/c/string/byte)
