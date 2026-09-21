# 17-8. 문자열 리터럴과 `const char *`

C17에서 ordinary string literal의 타입 규칙과 수정 가능성은 C++과 다르다. 읽기 전용 interface에는 `const char *`를 사용해 수정하지 않을 의도를 표현한다.

## 1. 학습 목표
- C17 string literal의 타입을 C++ 규칙과 구분한다.
- string literal을 수정하는 동작이 undefined behavior임을 설명한다.
- `const char *` 문자열 parameter를 array adjustment와 연결한다.

## 2. 선수 지식
Part 13의 null-terminated byte string과 Step 17-7의 읽기 전용 array parameter를 안다.

## 3. 핵심 개념
C17에서 `"hello"`와 같은 ordinary string literal은 `char`의 배열이다. C++처럼 일반적으로 `const char[N]` 타입이라고 설명하면 안 된다. 그러나 프로그램이 string literal의 element를 수정하려 하면 동작은 undefined behavior다.

따라서 수정하지 않을 pointer에는 다음처럼 의도를 타입에 담는다.

```c
const char *message = "hello";
```

`message`는 재지정할 수 있지만 `message`를 통한 character 수정은 허용되지 않는다.

## 4. 문법

```c
void print_text(const char text[]);
void print_text(const char *text);
```

function parameter 문맥에서 첫 선언의 array notation은 pointer parameter로 조정된다. 두 표기는 이 문맥에서 `const char *` parameter를 나타낸다.

표준 library의 대표적인 읽기 전용 interface도 같은 의도를 보인다.

```c
size_t strlen(const char *s);
```

반면 `strcpy`는 destination을 수정하고 source를 읽으므로 두 parameter의 qualifier 역할이 다르다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

static void print_text(const char text[])
{
    for (size_t i = 0; text[i] != '\0'; ++i) {
        putchar(text[i]);
    }
    putchar('\n');
}

int main(void)
{
    const char *message = "C17 const";

    print_text(message);
    return 0;
}
```

## 6. 코드 해석
1. `message`는 string literal의 첫 element를 가리키는 pointer value로 초기화된다.
2. `print_text`의 parameter는 조정 후 `const char *`다.
3. 함수는 null character까지 읽으며 character를 수정하지 않는다.
4. string literal 수정 같은 undefined behavior를 실행하지 않는다.

## 7. 내부 동작
- **[C17 표준]** ordinary string literal은 `char` 배열이며, 그 배열의 수명은 프로그램 실행 전체에 걸친다. element 수정 시도는 undefined behavior다.
- **[GCC 구현]** literal을 read-only section에 둘 수 있지만 section placement는 표준의 핵심 설명이 아니다.
- **[C++와 차이]** C++ string literal의 const-qualified array 타입 규칙을 C에 그대로 가져오면 안 된다.

## 8. 자주 하는 실수
- C string literal의 타입을 항상 `const char[N]`이라고 설명한다.
- `char *p = "text";`가 문법상 받아들여질 수 있다는 이유로 `p[0]` 수정도 안전하다고 생각한다.
- `const char *`가 pointer 자체도 고정한다고 생각한다.
- array parameter가 문자열 배열 전체를 복사한다고 생각한다.

## 9. 필수 실습
`print_text(const char text[])`를 작성해 string literal과 수정 가능한 `char` 배열을 각각 출력한다. 함수는 두 입력 모두 읽기만 한다.

실습 안내: [17-8 exercise](../../exercises/17-const-and-pointers/17-8/README.md)

## 10. 추가 실습
- ★ 기초: `strlen`으로 두 문자열 길이를 읽어 출력한다.
- ★★ 응용: `count_char(const char text[], char target)`를 작성한다.
- ★★★ 도전: destination 배열에 복사하는 함수 prototype에서 destination과 source의 qualifier 차이를 설명한다.

## 11. 확인 문제
1. C17 ordinary string literal의 배열 element type은 무엇인가?
2. string literal element를 수정하면 어떤 문제가 생기는가?
3. `const char *message`에서 pointer 자체는 재지정 가능한가?
4. `const char text[]` parameter는 어떤 type으로 조정되는가?
5. `strlen`의 parameter가 `const char *`인 이유는 무엇인가?
6. C와 C++의 string literal type 설명을 왜 구분해야 하는가?

## 12. 핵심 정리
- C17 ordinary string literal은 `char` 배열이지만 수정 동작은 undefined behavior다.
- 읽기 전용 문자열 접근에는 `const char *`가 의도를 명확히 한다.
- 문자열 array parameter에도 pointer adjustment가 적용된다.
- 실제 section placement는 compiler·linker·target 구현 문제다.

## 13. 다음 Step
[17-9. Part 17 종합 복습](17-9-part-17-review.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.4.5 String literals; 7.24.6.3 `strlen`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙과 library interface는 C17에서도 유지된다.
- [cppreference: string literal](https://en.cppreference.com/w/c/language/string_literal)
- [cppreference: `strlen`](https://en.cppreference.com/w/c/string/byte/strlen)
