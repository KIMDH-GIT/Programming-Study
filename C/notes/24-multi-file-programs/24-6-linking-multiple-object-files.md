# 24-6. 여러 object file link
## 1. 학습 목표
- 여러 object files의 references와 definitions를 link한다.
- one-command build와 separate compilation의 관계를 설명한다.
- compiler와 linker의 책임을 구분한다.
## 2. 선수 지식
24-5의 object file과 `-c` workflow를 안다.
## 3. 핵심 개념
linker는 각 object file의 external references를 compatible definitions와 연결하여 실행 파일을 만든다.

```text
main.o       ─ calculator_add reference ┐
calculator.o ─ calculator_add definition ├→ calculator_app
output.o     ─ output_result definition ┘
```

GCC driver에 여러 `.c`를 한 번에 주어도 각 translation unit이라는 개념이 사라지지 않는다. driver가 필요한 compile과 link 단계를 연속으로 orchestrate할 뿐이다.
## 4. 문법
```sh
gcc main.o calculator.o output.o -o calculator_app
```

또는:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c calculator.c output.c -o calculator_app
```
## 5. 최소 코드 예제
`calculator.h`
```c
int calculator_add(int lhs, int rhs);
```

`calculator.c`
```c
#include "calculator.h"

int calculator_add(int lhs, int rhs)
{
    return lhs + rhs;
}
```

`output.h`
```c
void output_result(int value);
```

`output.c`
```c
#include "output.h"

#include <stdio.h>

void output_result(int value)
{
    printf("result: %d\n", value);
}
```

`main.c`
```c
#include "calculator.h"
#include "output.h"

int main(void)
{
    output_result(calculator_add(8, 4));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c calculator.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c output.c
gcc main.o calculator.o output.o -o calculator_app
./calculator_app
```
## 6. 코드 해석
`main.o`는 두 public functions를 참조하고 다른 object files가 definitions를 제공한다. `printf` 같은 standard library function의 실제 연결은 C implementation과 toolchain이 제공하는 library 환경의 도움을 받는다.
## 7. 내부 동작
**[preprocessor]** 각 source가 필요한 declarations를 얻는다.

**[C translation unit]** 세 source는 합쳐지지 않고 별도로 번역된다.

**[compiler]** call site와 function bodies를 object code로 만든다.

**[linker]** external references와 definitions를 연결한다. linker는 C runtime function이 아니라 toolchain 단계다.

**[OS / loader]** executable format과 loading은 environment-specific이다.

**[CPU / ISA]** 최종 addresses와 instructions가 반영된 image를 실행한다.
## 8. 자주 하는 실수
- 한 명령 build가 모든 `.c`를 하나의 translation unit으로 만든다고 생각한다.
- linker가 source를 compile한다고 말한다.
- `#include`가 object file을 linker input에 추가한다고 말한다.
- object file 하나를 link 명령에서 빠뜨린다.
- ELF·PE·Mach-O 같은 특정 format을 C17 규칙으로 말한다.
## 9. 필수 실습
세 object files를 개별 생성하고 한 실행 파일로 link한다.
[24-6 exercise](../../exercises/24-multi-file-programs/24-6/README.md)
## 10. 추가 실습
- ★ 같은 program을 one-command build로 만든다.
- ★★ 한 object file을 빼고 diagnostic을 관찰한 뒤 정상 명령으로 복구한다.
- ★★★ 각 symbol의 reference와 definition 제공 파일을 표로 만든다.
## 11. 확인 문제
1. linker가 연결하는 두 종류의 정보는?
2. one-command build에서도 translation units는 분리되는가?
3. linker는 C source를 compile하는가?
4. `printf` implementation이 source repository에 없어도 되는 이유는?
5. executable format은 C17이 규정하는가?
## 12. 핵심 정리
- 여러 object files의 external references는 link에서 해결된다.
- GCC driver는 compile과 link를 orchestrate할 수 있다.
- language rules와 toolchain behavior를 구분한다.
## 13. 다음 Step
[24-7. external linkage와 `extern`](24-7-external-linkage-and-extern.md)
## 14. 참고 자료
- N1570 5.1.1.2, 6.2.2, 6.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Options for Linking](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)
- [cppreference: Storage-class specifiers](https://en.cppreference.com/w/c/language/storage_class_specifiers)
