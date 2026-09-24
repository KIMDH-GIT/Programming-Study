# 24-1. 기능별 source file 분리
## 1. 학습 목표
- 한 파일의 프로그램을 기능별 `.c`와 `.h`로 나눈다.
- 전처리, translation unit별 compile, link 단계를 구분한다.
- `#include`가 다른 `.c`를 연결하는 명령이 아님을 설명한다.
## 2. 선수 지식
Part 0의 compile 과정과 Part 10의 함수 선언·정의를 복습한다.
## 3. 핵심 개념
C17에는 공식 module 기능이 없다. 여기서 **모듈**은 관련 header와 source를 묶는 설계 관례다.

```text
main.c + calculator.h 내용
  ↓ preprocessing
main translation unit
  ↓ compilation
main.o

calculator.c + calculator.h 내용
  ↓ preprocessing
calculator translation unit
  ↓ compilation
calculator.o

main.o + calculator.o
  ↓ linker
calculator_app
```

header는 보통 `#include`로 각 translation unit에 포함되는 source text이며 자동으로 별도 compile되는 파일이 아니다.
## 4. 문법
```c
/* calculator.h: 공개 선언 */
int calculator_add(int lhs, int rhs);
```

```c
/* calculator.c: 구현 */
#include "calculator.h"

int calculator_add(int lhs, int rhs)
{
    return lhs + rhs;
}
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

`main.c`
```c
#include <stdio.h>

#include "calculator.h"

int main(void)
{
    printf("%d\n", calculator_add(7, 5));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c calculator.c -o calculator_app
./calculator_app
```
## 6. 코드 해석
`main.c`는 공개 선언만 사용하고 계산 구현은 `calculator.c`가 소유한다. 구현 source도 자신의 header를 include하므로 선언과 정의의 type이 다르면 같은 translation unit에서 compiler가 진단할 기회를 얻는다.
## 7. 내부 동작
**[preprocessor]** 두 `.c`의 `#include "calculator.h"`를 각각 처리한다.

**[C translation unit]** preprocessing 결과로 서로 독립적인 두 compilation 단위가 만들어진다.

**[compiler]** 각 translation unit을 target용 object code로 변환한다.

**[linker]** `main`에서 참조한 `calculator_add`를 구현의 외부 definition과 연결한다.

**[OS / loader]** 완성된 실행 파일을 process로 적재한다.

**[CPU / ISA]** 적재된 machine instructions를 실행한다. 이 분리는 특정 ISA 기능이 아니다.
## 8. 자주 하는 실수
- header도 `.c`처럼 별도 compile된다고 말한다.
- `#include "calculator.h"`가 `calculator.c`를 link한다고 생각한다.
- `#include "calculator.c"`를 일반적인 분리 방법으로 사용한다. 전처리 문법상 text inclusion은 가능하지만 중복 definition과 build 혼란을 만들기 쉬우므로 header 선언과 separate compilation을 사용한다.
- 파일만 나누고 공개 이름·책임을 정하지 않는다.
## 9. 필수 실습
덧셈 함수가 있는 단일 파일을 `main.c`, `calculator.c`, `calculator.h`로 나눈다.
[24-1 exercise](../../exercises/24-multi-file-programs/24-1/README.md)
## 10. 추가 실습
- ★ `calculator_subtract`를 추가한다.
- ★★ public 함수 이름에 `calculator_` prefix를 일관되게 붙인다.
- ★★★ 파일별 책임과 공개 interface를 표로 정리한다.
## 11. 확인 문제
1. 이번 Part에서 module이라는 말은 어떤 의미인가?
2. header가 일반적으로 별도 compile되지 않는 이유는?
3. `#include`와 linker의 역할 차이는?
4. 구현 `.c`도 자신의 header를 include하는 이유는?
5. `.c` include를 기본 구조로 권장하지 않는 이유는?
## 12. 핵심 정리
- 기능별 source 분리는 책임을 나누는 설계다.
- 각 `.c`는 preprocessing 후 독립적으로 compile된다.
- header 선언과 source 정의는 link 단계에서 완성된다.
## 13. 다음 Step
[24-2. declaration과 definition](24-2-declarations-and-definitions.md)
## 14. 참고 자료
- N1570 5.1.1.1, 5.1.1.2, 6.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C language declarations](https://en.cppreference.com/w/c/language/declarations)
- [GCC: Overall Options](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html)
