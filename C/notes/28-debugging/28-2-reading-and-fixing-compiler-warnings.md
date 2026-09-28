# 28-2. compiler warning 읽고 수정하기
## 1. 학습 목표
- warning의 위치·category·원인을 순서대로 읽는다.
- warning을 억제하지 않고 실제 결함을 수정한다.
- compiler warning, error, constraint diagnostic, link error, runtime error, UB를 구분한다.
## 2. 선수 지식
28-1의 warning options와 Part 27의 compile 성공·correctness 차이를 안다.
## 3. 핵심 개념
warning은 번역을 중단하지 않을 수도 있는 GCC diagnostic이다. error는 해당 invocation에서 translation 또는 link 성공을 막는 toolchain 결과다. C17 constraint violation에는 적어도 하나의 diagnostic이 요구되지만 exact wording과 warning/error severity는 ISO C가 정하지 않는다.

linker의 `undefined reference`, runtime의 nonzero exit, C17의 UB는 각각 다른 층이다.
## 4. 문법
⚠ 진단 관찰용 — 수정 전 source이며 실행하지 않는다.
```c
#include <stdio.h>

static void print_value(int value, int unused)
{
    printf("%d\n", value);
}

int main(void)
{
    print_value(42, 0);
    return 0;
}
```

GCC는 현재 version에서 unused parameter 관련 warning과 controlling option을 표시할 수 있다.
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic warning.c -o warning_app
```

원인이 잘못된 interface라면 parameter를 제거한다. 단순 cast나 `(void)unused;`로 warning만 숨기기 전에 contract를 먼저 고친다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static void print_value(int value)
{
    printf("%d\n", value);
}

int main(void)
{
    print_value(42);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o fixed_warning
./fixed_warning
```
## 6. 코드 해석
사용하지 않는 parameter를 interface에서 제거해 caller와 definition을 함께 수정했다. 출력은 `42`다.
## 7. 내부 동작
**[GCC]** diagnostic은 보통 source 위치, severity, 핵심 설명, controlling option을 제공한다. 전체 문장은 version별로 달라질 수 있으므로 category와 return status를 중심으로 검증한다.

**[C17]** warning이 가리키는 코드가 항상 UB인 것도, warning이 없는 코드가 항상 defined인 것도 아니다.

**[linker]** declaration만 있고 definition이 없을 때의 link error는 runtime UB와 다르다.
## 8. 자주 하는 실수
- 첫 warning을 읽지 않고 모든 줄을 무작정 바꾼다.
- cast나 suppression으로 type mismatch를 숨긴다.
- `-Werror`로 error가 된 warning을 C17 syntax error라고 부른다.
- 원하는 출력이 나왔다고 warning 원인이 해결됐다고 생각한다.
## 9. 필수 실습
unused parameter warning을 별도 diagnostic test로 관찰한 뒤 interface를 수정해 strict build를 통과시킨다.
[28-2 exercise](../../exercises/28-debugging/28-2/README.md)
## 10. 추가 실습
- ★ warning의 file·line·option을 기록한다.
- ★★ 첫 diagnostic부터 수정하는 이유를 설명한다.
- ★★★ warning·link error·runtime failure 사례를 분류한다.
## 11. 확인 문제
1. warning과 error의 차이는?
2. exact GCC diagnostic 문자열이 C17에 규정되는가?
3. cast로 warning을 없애면 bug도 해결되는가?
4. `undefined reference`는 UB인가?
5. warning 부재가 증명하지 못하는 것은?
## 12. 핵심 정리
- 위치와 category를 읽고 실제 contract를 수정한다.
- expected diagnostic은 정상 compile/run PASS와 분리한다.
- diagnostic과 C semantic category를 섞지 않는다.
## 13. 다음 Step
[28-3. `-g -Og` debug build](28-3-g-og-debug-build.md)
## 14. 참고 자료
- N1570 5.1.1.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
