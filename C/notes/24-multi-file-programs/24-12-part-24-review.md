# 24-12. Part 24 종합 복습
## 1. 학습 목표
- preprocessing부터 실행까지 multi-file pipeline을 종합한다.
- declaration, definition, scope, linkage, storage duration을 구분한다.
- 정상 build와 compile·link diagnostics를 분석한다.
## 2. 선수 지식
24-1부터 24-11까지를 학습했다.
## 3. 핵심 개념
```text
main.c + app.h
  ↓ preprocessing
main translation unit
  ↓ compile
main.o

app.c + app.h
  ↓ preprocessing
app translation unit
  ↓ compile
app.o

main.o + app.o
  ↓ link
app
  ↓ OS loader
process
  ↓ CPU
machine instructions 실행
```

`#include`는 source inclusion이고 link는 object code의 external references와 definitions를 연결한다.
## 4. 문법
```c
/* app.h */
int app_double(int value);

/* app.c */
#include "app.h"
static int normalize(int value);
int app_double(int value) { return normalize(value) * 2; }
static int normalize(int value) { return value < 0 ? -value : value; }
```
## 5. 최소 코드 예제
`app.h`
```c
int app_double_absolute(int value);
```

`app.c`
```c
#include "app.h"

static int absolute_value(int value)
{
    return value < 0 ? -value : value;
}

int app_double_absolute(int value)
{
    return absolute_value(value) * 2;
}
```

`main.c`
```c
#include <stdio.h>

#include "app.h"

int main(void)
{
    printf("%d\n", app_double_absolute(-6));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c app.c
gcc main.o app.o -o app
./app
```
## 6. 코드 해석
header는 public function declaration을 공유한다. implementation은 자신의 header를 include하고 private helper를 internal linkage로 제한한다. caller는 definition의 위치나 helper를 알 필요가 없다.
## 7. 내부 동작
**[preprocessor]** include·macro·conditional inclusion을 translation unit 생성 과정에서 처리한다. include guard의 구체적 작성은 Part 25에서 배운다.

**[C translation unit]** 하나의 `.c`를 중심으로 preprocessing된 compilation 단위이며 raw source file과 완전히 같지 않다.

**[compiler]** 각 translation unit의 syntax와 type compatibility를 검사하고 object code를 만든다.

**[linker]** external-linkage references와 definitions를 연결하고 누락·충돌을 진단한다.

**[OS / loader]** toolchain이 만든 executable을 process로 적재한다.

**[CPU / ISA]** 최종 machine instructions를 실행한다. source module 구조는 MIPS나 RISC-V 전용 기능이 아니다.

**[MIPS — 수업 기준]** 실제 call·address sequence는 compiler, ABI, linker 결과에 따라 달라진다.

**[RISC-V — 병행 학습]** 마찬가지로 symbol resolution을 단일 ISA instruction과 동일시하지 않는다.
## 8. 자주 하는 실수
- header를 별도 translation unit처럼 자동 compile한다고 말한다.
- `#include`가 function definition을 찾아 link한다고 말한다.
- translation unit을 raw `.c`와 완전히 동일시한다.
- scope, linkage, storage duration을 같은 속성으로 말한다.
- `static`을 값 유지로만, `extern`을 file import로만 설명한다.
- file-scope `int x;`를 declaration-only라고 말한다.
- C object와 `.o` object file을 같은 뜻으로 쓴다.
- include guard가 모든 linker duplicate definition을 해결한다고 생각한다.
## 9. 필수 실습
public API 하나와 private helper 하나를 가진 program을 separate compilation으로 build한다.
[24-12 exercise](../../exercises/24-multi-file-programs/24-12/README.md)
## 10. 추가 실습
- ★ one-command build도 실행해 결과를 비교한다.
- ★★ implementation object를 뺀 expected link failure를 분석한다.
- ★★★ 모든 public names, private names, C objects, object files를 분류한다.
## 11. 확인 문제
1. header는 일반적으로 어떻게 translation unit에 참여하는가?
2. declaration과 definition의 차이는?
3. scope와 linkage의 차이는?
4. external linkage와 internal linkage의 쓰임은?
5. block-scope static과 file-scope static은 무엇이 다른가?
6. undefined reference와 multiple definition은 어느 단계에서 관찰되는가?
7. C object와 object file의 차이는?
## 12. 핵심 정리
- preprocessing, translation unit compile, object-file link, loading, execution을 분리한다.
- public declarations는 header, definitions와 private helpers는 source에 둔다.
- language semantics와 GCC·linker·OS 관찰을 구분한다.
## 13. 다음 Step
Part 25의 첫 Step은 **25-1. 전처리 지시문과 결과**이다. 이번 Part에서는 Part 25 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 5.1.1.1, 5.1.1.2, 6.2.1, 6.2.2, 6.2.4, 6.7, 6.9, 6.10. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C language](https://en.cppreference.com/w/c/language)
- [GCC Online Documentation](https://gcc.gnu.org/onlinedocs/)
