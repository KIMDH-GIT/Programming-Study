# 25-8. header guard
## 1. 학습 목표
- include guard의 세 directives와 macro state를 설명한다.
- 같은 translation unit의 repeated inclusion을 막는다.
- include guard와 linker multiple-definition 문제를 구분한다.
## 2. 선수 지식
25-2 `#include`, 25-7 `#ifndef`, Part 24의 public header를 안다.
## 3. 핵심 개념
```c
#ifndef PROGRAMMING_STUDY_COMMON_H
#define PROGRAMMING_STUDY_COMMON_H

/* header contents */

#endif
```

첫 inclusion에서는 guard macro가 정의되지 않아 contents가 포함되고 macro가 정의된다. 같은 preprocessing translation unit에서 다시 include되면 controlling group이 제외된다.

다른 translation unit은 독립적으로 preprocessing하므로 같은 header를 각자 한 번씩 처리하는 것이 정상이다.
## 4. 문법
```c
#ifndef PROGRAMMING_STUDY_CALCULATOR_H
#define PROGRAMMING_STUDY_CALCULATOR_H

int calculator_add(int lhs, int rhs);

#endif
```

guard name은 project 안에서 충돌 가능성이 낮고 implementation reserved identifier가 아닌 이름을 사용한다. `__HEADER_H`나 `_Header_H` 같은 leading-underscore 형태를 임의로 권장하지 않는다.
## 5. 최소 코드 예제
`common.h`
```c
#ifndef PROGRAMMING_STUDY_COMMON_H
#define PROGRAMMING_STUDY_COMMON_H

struct CommonValue {
    int value;
};

#endif
```

`app.h`
```c
#ifndef PROGRAMMING_STUDY_APP_H
#define PROGRAMMING_STUDY_APP_H

#include "common.h"

int app_read(const struct CommonValue *item);

#endif
```

`app.c`
```c
#include "app.h"

int app_read(const struct CommonValue *item)
{
    return item->value;
}
```

`main.c`
```c
#include <stdio.h>

#include "app.h"
#include "common.h"

int main(void)
{
    const struct CommonValue item = {42};

    printf("%d\n", app_read(&item));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c app.c -o guard_app
./guard_app
```
## 6. 코드 해석
`main.c`는 `common.h`를 `app.h`를 통해 간접 include하고 다시 직접 include한다. guard 덕분에 같은 translation unit에서 `struct CommonValue` definition이 한 번만 처리된다.
## 7. 내부 동작
**[C translation phases / preprocessing]** `#ifndef`, `#define`, `#endif`가 해당 preprocessing translation unit의 macro state를 사용한다.

**[C compiler]** guard가 처리된 뒤 하나의 struct definition과 compatible declarations를 검사한다.

**[GCC driver / option]** strict C17 build로 nested/direct repeated include 구조를 검증한다.

**[build system]** 다른 `.c` translation unit도 자신의 preprocessing에서 header를 처리한다.

**[OS / CPU]** guard macro가 runtime global integer나 storage를 만들지 않는다.
## 8. 자주 하는 실수
- include guard가 program 전체에서 header를 한 번만 include한다고 말한다.
- linker multiple definition을 모두 guard가 해결한다고 말한다.
- header의 ordinary external definitions를 guard로 감싸면 여러 translation units 문제도 해결된다고 생각한다.
- reserved identifier 형태를 guard name으로 사용한다.
- guard가 circular type dependency까지 모두 해결한다고 생각한다.

`#pragma once`는 널리 지원되지만 ISO C17 standard preprocessing directive가 아니다. 기본 교재는 portable include guard pattern을 사용한다.
## 9. 필수 실습
`common.h`가 direct·indirect 두 경로로 포함되는 file set을 strict C17로 compile한다.
[25-8 exercise](../../exercises/25-preprocessor/25-8/README.md)
## 10. 추가 실습
- ★ guard macro 이름을 project prefix가 있는 이름으로 바꾼다.
- ★★ `main.c`와 `app.c`가 각각 header를 처리하는 흐름을 그린다.
- ★★★ guard가 해결하지 못하는 external definition 문제를 설명한다.
## 11. 확인 문제
1. include guard가 막는 repeated inclusion의 범위는?
2. 다른 translation unit에서 같은 header를 include해도 되는가?
3. 세 directives는 macro state를 어떻게 이용하는가?
4. include guard가 linker duplicate definition을 해결하는가?
5. leading underscore guard 이름을 피하는 이유는?
6. `#pragma once`는 ISO C17 directive인가?
## 12. 핵심 정리
- guards는 같은 preprocessing translation unit의 repeated header contents를 제한한다.
- 각 translation unit은 독립적인 macro state로 preprocessing된다.
- guard와 linkage·definition 문제를 구분한다.
## 13. 다음 Step
[25-9. macro·함수·상수 선택](25-9-choosing-macros-functions-and-constants.md)
## 14. 참고 자료
- N1570 6.4.2.1, 6.10.1, 6.10.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Header guard](https://en.cppreference.com/w/c/preprocessor/conditional)
- [GCC: Once-Only Headers](https://gcc.gnu.org/onlinedocs/cpp/Once-Only-Headers.html)
