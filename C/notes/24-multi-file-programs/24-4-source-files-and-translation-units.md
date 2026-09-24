# 24-4. source file과 translation unit
## 1. 학습 목표
- source file과 translation unit을 구분한다.
- 한 `.c`를 중심으로 한 preprocessing 결과를 compilation 단위로 설명한다.
- preprocessing, compiler, linker, loader, CPU 층을 분리한다.
## 2. 선수 지식
24-1부터 24-3까지와 Part 0의 전처리 과정을 안다.
## 3. 핵심 개념
입문 수준에서 translation unit은 “하나의 `.c`를 중심으로 header inclusion과 preprocessing을 거친 compilation 단위”라고 설명할 수 있다. 원본 `.c` 파일과 완전히 같은 text는 아니다.

```text
main.c ─┐
config.h ─ include·macro 처리 → main translation unit

report.c ─┐
config.h  ─ include·macro 처리 → report translation unit
```

같은 header가 두 translation units에 각각 포함되는 것은 정상이다.
## 4. 문법
```c
#include "message.h"
```

이 directive는 preprocessing에 참여한다. function definition이 어느 `.c`에 있는지 linker에게 직접 지정하지 않는다.
## 5. 최소 코드 예제
`message.h`
```c
const char *message_text(void);
```

`message.c`
```c
#include "message.h"

const char *message_text(void)
{
    return "two translation units";
}
```

`main.c`
```c
#include <stdio.h>

#include "message.h"

int main(void)
{
    puts(message_text());
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c message.c -o message_app
./message_app
```
## 6. 코드 해석
`main.c`와 `message.c`는 각각 `message.h` 내용을 포함한 뒤 독립 translation unit으로 compile된다. 두 `.c`가 전처리로 하나의 거대한 source가 되는 것이 아니다.
## 7. 내부 동작
**[preprocessor]** inclusion과 macro replacement 등 preprocessing directives를 처리한다.

**[C translation unit]** preprocessing tokens와 declarations가 한 compilation 단위를 형성한다.

**[compiler]** 다른 `.c`를 자동 검색하지 않고 주어진 translation unit을 번역한다.

**[linker]** object code의 external references와 definitions를 연결한다.

**[OS / loader]** 실행 image를 memory에 배치한다. format은 OS·toolchain에 의존한다.

**[CPU / ISA]** 최종 instructions를 실행한다. symbol resolution 자체를 특정 `jal` 같은 한 instruction으로 설명할 수 없다.
## 8. 자주 하는 실수
- translation unit을 원본 `.c` 한 파일과 완전히 동일시한다.
- compiler가 call을 보면 다른 `.c`를 자동으로 찾는다고 말한다.
- linker가 C source를 compile한다고 말한다.
- include guard가 프로그램 전체에서 header를 한 번만 허용한다고 생각한다. guard는 Part 25에서 같은 preprocessing translation unit의 repeated inclusion 방지로 배운다.
## 9. 필수 실습
preprocessing 결과와 두 translation units의 관계를 그림으로 설명하고 프로그램을 build한다.
[24-4 exercise](../../exercises/24-multi-file-programs/24-4/README.md)
## 10. 추가 실습
- ★ `gcc -E main.c` 출력에서 header declaration을 찾는다.
- ★★ `message.c`의 preprocessing 결과와 비교한다.
- ★★★ header 변경 시 다시 compile해야 하는 translation units를 표시한다.
## 11. 확인 문제
1. source file과 translation unit의 차이는?
2. 같은 header가 두 translation units에 포함되어도 되는가?
3. compiler가 다른 `.c`를 자동 검색하는가?
4. linker 입력은 보통 source text인가 object code인가?
5. OS loader와 CPU는 어느 시점에 관여하는가?
## 12. 핵심 정리
- translation unit은 preprocessing 결과를 포함한 compilation 단위다.
- `.c`별 translation units는 독립적으로 compile된다.
- external 연결은 preprocessing이 아니라 link 단계의 일이다.
## 13. 다음 Step
[24-5. 개별 compile과 object file](24-5-separate-compilation-and-object-files.md)
## 14. 참고 자료
- N1570 5.1.1.1, 5.1.1.2, 6.10. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Phases of translation](https://en.cppreference.com/w/c/language/translation_phases)
- [GCC: Preprocessor Options](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html)
