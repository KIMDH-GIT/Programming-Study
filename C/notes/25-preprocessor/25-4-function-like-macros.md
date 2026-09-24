# 25-4. 함수형 macro
## 1. 학습 목표
- function-like macro의 invocation과 expansion을 설명한다.
- macro parameters와 function parameters를 구분한다.
- expansion 이후 type checking이 일어남을 이해한다.
## 2. 선수 지식
25-3 object-like macro와 Part 10 function을 안다.
## 3. 핵심 개념
```c
#define ADD(lhs, rhs) ((lhs) + (rhs))
```

`lhs`, `rhs`는 runtime parameter objects가 아니라 replacement에 쓰이는 macro parameters다. type과 storage가 없다. compiler는 expansion 뒤 생성된 C expression을 검사한다.

function-like macro name 뒤에 다음 preprocessing token으로 `(`가 와야 invocation으로 인식된다.
## 4. 문법
```c
#define ADD(lhs, rhs) ((lhs) + (rhs))
```

definition에서 macro name과 `(` 사이에는 whitespace를 두지 않는다. 사용에서는:
```c
int sum = ADD(2, 3);
```
처럼 호출 모양을 갖지만 C function call과 같은 mechanism은 아니다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define ADD(lhs, rhs) ((lhs) + (rhs))

int main(void)
{
    const int sum = ADD(2, 3);

    printf("%d\n", sum);
    return 0;
}
```

```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o function_macro_app
./function_macro_app
```
## 6. 코드 해석
preprocessing은 `ADD(2, 3)`을 괄호가 있는 addition expression tokens로 확장한다. runtime에는 `ADD` function이나 parameter objects가 존재하지 않는다.
## 7. 내부 동작
**[C translation phases / preprocessing]** invocation arguments가 macro replacement rules에 따라 parameters를 대신한다.

**[C compiler]** 확장된 `((2) + (3))` expression의 operand types와 result type을 결정한다.

**[GCC driver / option]** `-E` 출력으로 macro invocation이 사라진 결과를 관찰할 수 있다.

**[build system]** macro definitions가 header에 있다면 이를 include하는 모든 translation units에 영향을 줄 수 있다.

**[OS / CPU]** function-like macro 자체를 호출하지 않는다. expansion에서 생성된 C expression에 대응하는 code만 실행한다.
## 8. 자주 하는 실수
- function-like macro를 C function이라고 부른다.
- macro parameters에 runtime type과 storage가 있다고 생각한다.
- macro 자체에 fixed return type이 있다고 말한다.
- definition name과 `(` 사이에 whitespace를 넣어 object-like macro를 만든다.
- side effects가 있는 arguments를 안전한 function argument처럼 사용한다.
## 9. 필수 실습
side-effect-free integer arguments로 `ADD` macro를 사용하고 expansion 결과를 확인한다.
[25-4 exercise](../../exercises/25-preprocessor/25-4/README.md)
## 10. 추가 실습
- ★ 두 `double` literals에 `ADD`를 사용해 expansion 뒤 type을 관찰한다.
- ★★ 같은 기능의 `int add(int, int)` function과 비교한다.
- ★★★ macro name collision을 줄이는 project prefix를 설계한다.
## 11. 확인 문제
1. function-like macro invocation 조건은?
2. macro parameter가 runtime object인가?
3. macro의 argument·result type은 언제 결정되는가?
4. function call과 macro expansion의 차이는?
5. generic function처럼 안전하다고 볼 수 없는 이유는?
## 12. 핵심 정리
- function-like macro는 preprocessing mechanism이다.
- parameters에는 C type이나 storage가 없다.
- expansion 뒤 expression을 compiler가 검사한다.
## 13. 다음 Step
[25-5. `SQUARE(x)`와 괄호](25-5-square-and-parentheses.md)
## 14. 참고 자료
- N1570 6.10.3, 6.10.3.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Replacing text macros](https://en.cppreference.com/w/c/preprocessor/replace)
- [GCC: Macro Arguments](https://gcc.gnu.org/onlinedocs/cpp/Macro-Arguments.html)
