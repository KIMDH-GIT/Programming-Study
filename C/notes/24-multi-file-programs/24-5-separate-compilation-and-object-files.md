# 24-5. 개별 compile과 object file
## 1. 학습 목표
- 각 translation unit을 개별 compile한다.
- GCC `-c`와 Unix-like 환경의 `.o` artifact를 설명한다.
- C object와 object file을 구분한다.
## 2. 선수 지식
24-4 translation unit과 Part 0의 compiler·assembler 단계를 안다.
## 3. 핵심 개념
separate compilation은 각 translation unit을 독립적으로 object file로 만든 뒤 나중에 link하는 방식이다.

```text
main.c     → main.o
counter.c  → counter.o
```

GCC에서 `-c`는 compile·assemble까지 수행하고 link는 수행하지 않는 driver option이다. `.o` 확장자는 Unix-like GCC 환경의 관례이며 ISO C17이 filename이나 object-file format을 규정하지 않는다.
## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c counter.c
```
## 5. 최소 코드 예제
`counter.h`
```c
int counter_next(int value);
```

`counter.c`
```c
#include "counter.h"

int counter_next(int value)
{
    return value + 1;
}
```

`main.c`
```c
#include <stdio.h>

#include "counter.h"

int main(void)
{
    printf("%d\n", counter_next(9));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c counter.c
gcc main.o counter.o -o counter_app
./counter_app
```
## 6. 코드 해석
첫 두 명령은 link하지 않으므로 각 source를 독립적으로 확인할 수 있다. 마지막 GCC invocation은 object files를 linker에 전달해 실행 파일을 만든다.
## 7. 내부 동작
**[preprocessor]** 각 source의 header inclusion을 따로 처리한다.

**[C translation unit]** `main`과 `counter_next` definition은 서로 다른 단위에 있다.

**[compiler / assembler]** GCC driver가 translation unit을 target object code가 든 artifact로 만든다.

**[linker]** 이 Step의 마지막 명령에서 두 object files를 결합한다.

**[OS / loader]** `counter_app`을 실행할 때 관여한다.

**[CPU / ISA]** object files 자체가 아니라 적재된 실행 image의 instructions를 실행한다.
## 8. 자주 하는 실수
- `-c`를 C language syntax라고 말한다.
- `.o` 확장자를 C17이 보장한다고 말한다.
- source의 `int value` 같은 C object를 object file이라고 부른다.
- compile-only 성공을 전체 build 성공으로 생각한다.
## 9. 필수 실습
두 source를 각각 `-c`로 compile한 뒤 object files를 link한다.
[24-5 exercise](../../exercises/24-multi-file-programs/24-5/README.md)
## 10. 추가 실습
- ★ `-o main_part.o`처럼 output 이름을 명시한다.
- ★★ `counter.c`만 수정한 뒤 어떤 translation unit만 다시 compile할지 설명한다.
- ★★★ source, translation unit, C object, object file을 비교한다.
## 11. 확인 문제
1. GCC `-c`는 어느 단계를 생략하는가?
2. `.o` 확장자는 ISO C17 보장인가?
3. C object와 object file의 차이는?
4. compile-only 성공 뒤에도 link가 실패할 수 있는 이유는?
5. separate compilation의 장점은?
## 12. 핵심 정리
- `-c`는 GCC driver의 compile-only workflow에 쓰인다.
- object file은 build artifact이고 C object는 C abstract machine의 data object다.
- 개별 compile 후 별도 link할 수 있다.
## 13. 다음 Step
[24-6. 여러 object file link](24-6-linking-multiple-object-files.md)
## 14. 참고 자료
- N1570 3.14, 5.1.1.1, 5.1.1.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Overall Options (`-c`)](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html)
- [cppreference: Object](https://en.cppreference.com/w/c/language/object)
