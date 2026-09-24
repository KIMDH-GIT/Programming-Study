# 25-10. Part 25 종합 복습
## 1. 학습 목표
- preprocessing, compilation, link, runtime을 종합해 구분한다.
- include guard, macros, conditional compilation을 안전하게 사용한다.
- C17과 later-standard·implementation extensions의 경계를 확인한다.
## 2. 선수 지식
25-1부터 25-9까지를 학습했다.
## 3. 핵심 개념
```text
source + included headers
  ↓ preprocessing directives·macro expansion
preprocessed translation unit
  ↓ compiler·assembler
object file
  ↓ linker
executable
  ↓ OS loader·CPU
execution
```

macro는 preprocessing tokens를 다루며 variable·function·runtime branch와 동일하지 않다.
## 4. 문법
```c
#ifndef PROGRAMMING_STUDY_CONFIG_H
#define PROGRAMMING_STUDY_CONFIG_H

#ifndef APP_DEBUG
#define APP_DEBUG 0
#endif

#define SQUARE(x) ((x) * (x))

#endif
```
## 5. 최소 코드 예제
`config.h`
```c
#ifndef PROGRAMMING_STUDY_CONFIG_H
#define PROGRAMMING_STUDY_CONFIG_H

#ifndef APP_DEBUG
#define APP_DEBUG 0
#endif

#define SQUARE(x) ((x) * (x))

#endif
```

`main.c`
```c
#include <stdio.h>

#include "config.h"

int main(void)
{
    const int value = 4;

#if APP_DEBUG
    puts("debug");
#else
    puts("release");
#endif
    printf("%d\n", SQUARE(value));
    return 0;
}
```

두 configuration build:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o review_release
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -DAPP_DEBUG=1 main.c -o review_debug
./review_release
./review_debug
```
## 6. 코드 해석
guard는 같은 translation unit의 repeated header inclusion을 제한한다. `APP_DEBUG`는 command line에서 override 가능한 compile-time configuration이고 `SQUARE`에는 side-effect-free object만 전달한다.
## 7. 내부 동작
**[C translation phases / preprocessing]** inclusion, conditional groups, macro definitions와 invocations를 처리한다.

**[C compiler]** 각 configuration에서 선택·확장된 C program을 type-check하고 code를 생성한다.

**[GCC driver / option]** `-E`와 `-D`는 GCC options다. `#pragma once`와 `#warning`은 ISO C17 directives가 아니다.

**[build system]** release와 debug configurations를 각각 compile해야 한다.

**[OS / CPU]** preprocessing directives 자체가 runtime instructions로 실행되지는 않는다.

C17 범위 밖 기능을 섞지 않는다. `#elifdef`, `#elifndef`, `__VA_OPT__`는 C17 기능이 아니다. GNU `typeof`, statement expression `({ ... })`, `__auto_type`, `, ##__VA_ARGS__`도 portable ISO C17 기본 예제로 사용하지 않는다.
## 8. 자주 하는 실수
- preprocessor를 runtime component로 설명한다.
- macro를 typed variable이나 function으로 설명한다.
- `#include`를 linker 명령으로 설명한다.
- guard가 program 전체 inclusion이나 linker definitions를 해결한다고 말한다.
- parentheses가 multiple evaluation까지 해결한다고 생각한다.
- one configuration의 build로 모든 conditional groups가 검증됐다고 생각한다.
- implementation extension이나 C23 feature를 C17이라고 말한다.
## 9. 필수 실습
guarded config header를 release·debug 두 configurations로 build·실행하고 preprocessing 결과도 확인한다.
[25-10 exercise](../../exercises/25-preprocessor/25-10/README.md)
## 10. 추가 실습
- ★ `SQUARE(5)`를 side-effect-free input으로 확인한다.
- ★★ `gcc -E -DAPP_DEBUG=1 main.c`에서 선택된 group을 찾는다.
- ★★★ macro·function·enum·`const` 선택표를 자신의 말로 작성한다.
## 11. 확인 문제
1. preprocessing과 runtime의 경계는?
2. include guard의 적용 범위는?
3. macro와 typed function의 차이는?
4. `SQUARE(i++)`가 위험한 이유는?
5. conditional configurations를 각각 build해야 하는 이유는?
6. `#pragma once`는 ISO C17 directive인가?
7. `__VA_OPT__`와 `#elifdef`는 C17 기능인가?
## 12. 핵심 정리
- directives와 macros는 translation 과정에서 처리된다.
- include guards, parentheses, side-effect-free arguments를 목적에 맞게 사용한다.
- C17, later standards, GCC extensions를 구분한다.
## 13. 다음 Step
Part 26의 첫 Step은 **26-1. C storage duration과 OS 배치의 구분**이다. 이번 Part에서는 Part 26 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 5.1.1.2, 6.10.1~6.10.3, 6.10.8. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Preprocessor](https://en.cppreference.com/w/c/preprocessor)
- [GCC: The C Preprocessor](https://gcc.gnu.org/onlinedocs/cpp/)
