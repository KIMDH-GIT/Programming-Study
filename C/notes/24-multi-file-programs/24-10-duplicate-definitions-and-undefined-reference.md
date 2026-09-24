# 24-10. 중복 정의와 undefined reference
## 1. 학습 목표
- compile diagnostic과 link diagnostic을 구분한다.
- undefined reference와 multiple definition의 구조적 원인을 찾는다.
- 의도적 failure를 안전하게 관찰하고 정상 build와 분리한다.
## 2. 선수 지식
24-2 declaration·definition, 24-6 link, 24-7 external linkage를 안다.
## 3. 핵심 개념
compiler는 현재 translation unit에 compatible declaration이 있으면 call을 compile할 수 있다. 그러나 definition이 든 object file을 link하지 않으면 external reference를 해결하지 못한다.

```text
compile-time 예: syntax error, undeclared identifier, conflicting types
link-time 예: unresolved external symbol, duplicate external definition
```

`undefined reference`, `multiple definition`은 흔한 GCC/linker diagnostic wording이며 ISO C17이 exact message를 규정하지 않는다.
## 4. 문법
정상 구조:
```c
/* math_utils.h */
int add(int lhs, int rhs);

/* math_utils.c */
#include "math_utils.h"
int add(int lhs, int rhs) { return lhs + rhs; }
```

잘못된 구조는 같은 external-linkage function definition을 두 translation units에 만드는 것이다.
## 5. 최소 코드 예제
`math_utils.h`
```c
int add(int lhs, int rhs);
```

`math_utils.c`
```c
#include "math_utils.h"

int add(int lhs, int rhs)
{
    return lhs + rhs;
}
```

`main.c`
```c
#include <stdio.h>

#include "math_utils.h"

int main(void)
{
    printf("%d\n", add(3, 4));
    return 0;
}
```

정상 build:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c math_utils.c -o math_app
./math_app
```

expected link failure 관찰:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc main.o -o missing_definition_app
```
두 번째 명령은 `add` definition을 제공하는 object file이 없으므로 현재 toolchain에서 link failure가 예상된다.
## 6. 코드 해석
`main.c` compile은 header의 compatible declaration 덕분에 성공할 수 있다. 정상 build는 `math_utils.c`의 definition까지 link한다. 실패 관찰은 정상 executable 검증과 별도 명령으로 수행한다.
## 7. 내부 동작
**[preprocessor]** declaration을 각 translation unit에 포함한다.

**[C translation unit]** compiler는 syntax, identifiers, type compatibility를 검사한다.

**[compiler]** declaration을 근거로 unresolved external call이 있는 object code를 만들 수 있다.

**[linker]** definition 누락 또는 충돌을 발견한다. compiler가 다른 `.c`를 자동 검색해 해결하지 않는다.

**[OS / loader]** link가 실패하면 정상 executable이 없으므로 실행 검증 단계로 가지 않는다.

**[CPU / ISA]** link diagnostic은 CPU runtime failure가 아니다.
## 8. 자주 하는 실수
- declaration이 있으므로 definition도 있다고 생각한다.
- link failure를 syntax error라고 부른다.
- header에 function body나 initialized global을 넣어 duplicate definitions를 만든다.
- source file을 link 명령에서 누락한다.
- include guard가 linker의 multiple-definition 문제를 모두 해결한다고 말한다.
## 9. 필수 실습
정상 build를 먼저 통과시킨 뒤 implementation object를 뺀 expected link failure를 별도로 관찰한다.
[24-10 exercise](../../exercises/24-multi-file-programs/24-10/README.md)
## 10. 추가 실습
- ★ implementation source를 다시 포함해 정상 link한다.
- ★★ 두 source에 같은 external function definition을 만들어 diagnostic을 관찰한 뒤 삭제한다.
- ★★★ compile 단계와 link 단계의 입력·오류를 표로 정리한다.
## 11. 확인 문제
1. declaration만 있어도 function call compile이 가능한 이유는?
2. definition을 link하지 않으면 어느 단계가 실패하는가?
3. multiple definition의 일반적인 원인은?
4. include guard가 모든 linker 중복 정의를 해결하는가?
5. diagnostic exact wording은 C17 보장인가?
6. expected failure를 정상 PASS count와 분리하는 이유는?
## 12. 핵심 정리
- compile은 translation unit의 문법·type을, link는 external 연결을 검사한다.
- undefined reference는 definition 누락을 먼저 의심한다.
- multiple definition은 external definition 배치를 점검한다.
## 13. 다음 Step
[24-11. 학생 관리 프로그램 파일 분리](24-11-splitting-student-management-program.md)
## 14. 참고 자료
- N1570 5.1.1.1, 5.1.1.2, 6.2.2, 6.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
- [GCC: Options for Linking](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)
