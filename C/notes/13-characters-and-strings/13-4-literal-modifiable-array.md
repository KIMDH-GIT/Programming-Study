# 13-4. 문자열 리터럴과 수정 가능한 배열

string literal로 char array를 초기화하면 array elements는 수정할 수 있지만 literal 자체를 수정 가능한 객체처럼 취급하면 안 된다.

## 1. 학습 목표
- string literal과 그 값으로 초기화된 char array를 구분한다.
- char array element를 안전하게 수정한다.
- literal 수정의 pointer 규칙은 후속 Part로 제한한다.

## 2. 선수 지식
Step 13-2의 string literal과 Part 11의 element assignment를 안다.

## 3. 핵심 개념
`char text[] = "cat";`은 네 elements `{'c','a','t','\0'}`를 가진 array object를 만든다. `text[0] = 'C';`는 그 array element를 수정하므로 결과 string은 `"Cat"`이다. string literal은 수정 가능한 일반 array object와 같은 사용 규칙을 갖지 않으며 pointer로 literal을 가리키고 수정하는 자세한 내용은 Part 14 이후로 미룬다.

## 4. 문법
```c
char text[] = "cat";
text[0] = 'C';
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    char text[] = "cat";

    printf("%s\n", text);
    text[0] = 'C';
    printf("%s\n", text);
    return 0;
}
```

## 6. 코드 해석
초기 array는 `cat`과 terminator를 가진다. index 0 element만 소문자 c에서 대문자 C로 바뀌며 나머지 elements와 terminator는 유지되어 `Cat`을 안전하게 출력한다.

## 7. 내부 동작
[C17 표준] string literal을 수정하려는 동작은 Undefined Behavior다. 이 예제는 literal을 직접 수정하지 않고 literal로 초기화된 별도 char array를 수정한다. 배열과 pointer의 차이는 Part 15에서 더 정확히 다룬다.

## 8. 자주 하는 실수
- char array와 string literal을 같은 객체라고 생각한다.
- terminator element를 일반 문자로 덮어쓴다.
- pointer 문법으로 literal 수정 예제를 만들어 실행한다.
- array element 수정과 array 전체 assignment를 혼동한다.

## 9. 필수 실습
`"hello"`로 초기화한 char array의 첫 element를 대문자로 바꿔 출력한다. [실습 README](../../exercises/13-characters-and-strings/13-4/README.md)

## 10. 추가 실습
- ★ 마지막 표시 문자 수정
- ★★ 두 positions를 안전하게 수정
- ★★★ literal과 modifiable array 차이 표 작성

## 11. 확인 문제
1. `char text[] = "cat";`의 element count는?
2. `text[0]`은 수정 가능한가?
3. terminator를 유지해야 하는 이유는?
4. string literal 자체 수정의 C17 분류는?
5. 배열과 pointer가 같은 개념인가?

## 12. 핵심 정리
literal로 초기화한 char array는 별도 수정 가능한 object이며 null terminator를 보존해 string 상태를 유지한다.

## 13. 다음 Step
[Step 13-5. 제한된 문자열 입력과 잘린 줄 처리](13-5-bounded-input-truncated-line.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.4.5, 6.7.9
- [cppreference: String literal](https://en.cppreference.com/w/c/language/string_literal.html)
