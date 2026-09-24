# 25-2. `#include`
## 1. 학습 목표
- `#include`를 source inclusion directive로 설명한다.
- quote form과 angle form의 search 차이를 정확히 표현한다.
- header inclusion과 linking을 구분한다.
## 2. 선수 지식
25-1 preprocessing 결과와 Part 24의 header·translation unit을 안다.
## 3. 핵심 개념
```c
#include "greeting.h"
```
는 `greeting.c`를 linker에 추가하라는 명령이 아니다. preprocessing 과정에서 지정된 header의 contents가 directive 위치에서 처리되도록 한다.

```text
main.c + included greeting.h
  ↓ preprocessing
main translation unit
```

header 자체가 일반적으로 독립 translation unit처럼 자동 compile되는 것은 아니다.
## 4. 문법
```c
#include <stdio.h>
#include "greeting.h"
```

angle form은 implementation이 정한 방식으로 header를 찾는다. quote form은 먼저 implementation-defined 방식으로 source file 주변 등에서 찾고, 실패하면 angle form과 같은 방식으로 다시 찾는다. 이를 “현재 directory”와 “`/usr/include`”로 고정하면 안 된다.
## 5. 최소 코드 예제
`greeting.h`
```c
#define GREETING "hello from a header"
```

`main.c`
```c
#include <stdio.h>

#include "greeting.h"

int main(void)
{
    puts(GREETING);
    return 0;
}
```

```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o greeting_app
./greeting_app
```
## 6. 코드 해석
`greeting.h`의 macro definition은 `main.c` 기반 preprocessing translation unit에 들어온다. 별도의 `greeting.o`가 만들어지지 않으며 이 예제에는 다른 source definition을 link할 필요도 없다.
## 7. 내부 동작
**[C translation phases / preprocessing]** header-name preprocessing token sequence에 따라 header가 검색되고 contents가 처리된다.

**[C compiler]** inclusion과 macro expansion 뒤의 translation unit을 검사한다.

**[GCC driver / option]** `-I` 같은 search-path option은 GCC/build environment 기능이며 C source syntax가 아니다.

**[build system]** header 변경 시 그 header를 포함한 translation units를 다시 compile해야 할 수 있다.

**[OS / CPU]** header file을 runtime에 별도 loading하거나 실행하지 않는다.
## 8. 자주 하는 실수
- `#include`가 다른 `.c` definition을 link한다고 말한다.
- header도 `.c`와 같이 자동으로 object file이 된다고 생각한다.
- quote include는 무조건 현재 directory만 찾는다고 단정한다.
- angle include는 무조건 `/usr/include`만 찾는다고 단정한다.
- 필요한 standard header를 transitive include에 의존해 생략한다.
## 9. 필수 실습
작은 project header를 quote form으로 include하고 preprocessing 결과를 확인한다.
[25-2 exercise](../../exercises/25-preprocessor/25-2/README.md)
## 10. 추가 실습
- ★ `GREETING` replacement를 바꾼다.
- ★★ `gcc -E`에서 header content가 들어온 위치를 찾는다.
- ★★★ header search가 implementation·build environment에 의존하는 이유를 정리한다.
## 11. 확인 문제
1. `#include`는 어느 단계에서 처리되는가?
2. `#include "greeting.h"`가 `greeting.c`를 link하는가?
3. header는 일반적으로 별도 translation unit으로 compile되는가?
4. quote form과 angle form을 directory 하나로 고정할 수 없는 이유는?
5. transitive include 의존이 취약한 이유는?
## 12. 핵심 정리
- `#include`는 preprocessing source inclusion이다.
- header contents는 각 translation unit에서 처리된다.
- header search와 link 입력 선택은 서로 다른 문제다.
## 13. 다음 Step
[25-3. 객체형 `#define`](25-3-object-like-define.md)
## 14. 참고 자료
- N1570 6.10.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Source file inclusion](https://en.cppreference.com/w/c/preprocessor/include)
- [GCC: Search Path](https://gcc.gnu.org/onlinedocs/cpp/Search-Path.html)
