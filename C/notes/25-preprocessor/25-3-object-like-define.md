# 25-3. 객체형 `#define`
## 1. 학습 목표
- object-like macro의 name과 replacement list를 구분한다.
- macro, variable, `const` object, enum constant를 비교한다.
- expansion 뒤 compiler가 보는 token sequence를 설명한다.
## 2. 선수 지식
25-1 preprocessing과 Part 17 `const`, Part 20 `enum`을 안다.
## 3. 핵심 개념
```c
#define BUFFER_SIZE 128
```

`BUFFER_SIZE`는 macro name이고 `128`은 replacement list다. 이 directive만으로 C object나 storage가 생기지 않는다.

```c
int values[BUFFER_SIZE];
```
는 preprocessing 뒤 compiler가 개념적으로 `int values[128];`에 해당하는 token sequence를 보게 한다.
## 4. 문법
```c
#define APP_NAME "study"
#define BUFFER_SIZE 128
```

나쁜 예:
```c
#define BUFFER_SIZE 128;
```
사용 위치에 불필요한 `;` token까지 들어가므로 constant macro replacement에 semicolon을 넣지 않는다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define APP_NAME "preprocessor study"
#define BUFFER_SIZE 4

int main(void)
{
    int values[BUFFER_SIZE] = {10, 20, 30, 40};

    printf("%s: %zu elements\n",
           APP_NAME,
           sizeof values / sizeof values[0]);
    return 0;
}
```

```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o object_macro_app
./object_macro_app
```
## 6. 코드 해석
`APP_NAME`과 `BUFFER_SIZE`는 preprocessing tokens를 제공한다. compiler는 expansion 결과의 string literal과 integer constant expression을 검사한다.
## 7. 내부 동작
**[C translation phases / preprocessing]** macro name이 활성화된 범위에서 preprocessing token replacement가 일어난다.

**[C compiler]** macro의 type을 검사하는 것이 아니라 expansion 뒤 만들어진 declarations와 expressions의 C type·constraints를 검사한다.

**[GCC driver / option]** `-E`로 `BUFFER_SIZE`가 사라지고 replacement tokens가 들어간 결과를 관찰할 수 있다.

**[build system]** command-line `-D`도 macro definition을 제공할 수 있지만 이 Step의 source example에는 필요하지 않다.

**[OS / CPU]** macro name용 runtime storage나 global integer가 자동 생성되지 않는다.
## 8. 자주 하는 실수
- `#define`이 variable에 값을 저장한다고 말한다.
- macro 자체에 `int`나 `double` type이 있다고 생각한다.
- `#define PI 3.14`가 `double` object를 정의한다고 말한다.
- replacement list에 불필요한 semicolon을 넣는다.
- 모든 named constant를 무조건 macro로 만들어야 한다고 생각한다.
## 9. 필수 실습
문자열 macro와 배열 크기 macro를 정의하고 preprocessing·실행 결과를 확인한다.
[25-3 exercise](../../exercises/25-preprocessor/25-3/README.md)
## 10. 추가 실습
- ★ `BUFFER_SIZE`와 initializer를 5개로 바꾼다.
- ★★ `enum { BUFFER_SIZE = 4 };` 방식과 비교한다.
- ★★★ block-scope `const int` object와 storage·type 관점에서 비교한다.
## 11. 확인 문제
1. macro name과 replacement list는 각각 무엇인가?
2. object-like macro가 C object를 생성하는가?
3. macro에 C data type이 있는가?
4. enum constant와 macro는 어느 단계에서 다르게 처리되는가?
5. replacement list의 semicolon이 위험한 이유는?
## 12. 핵심 정리
- object-like macro는 preprocessing token replacement를 정의한다.
- macro는 variable, `const` object, enum constant와 다른 mechanism이다.
- compiler는 expansion 결과를 type-check한다.
## 13. 다음 Step
[25-4. 함수형 macro](25-4-function-like-macros.md)
## 14. 참고 자료
- N1570 6.10.3, 6.10.3.4. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: `#define`](https://en.cppreference.com/w/c/preprocessor/replace)
- [cppreference: Constant expression](https://en.cppreference.com/w/c/language/constant_expression)
