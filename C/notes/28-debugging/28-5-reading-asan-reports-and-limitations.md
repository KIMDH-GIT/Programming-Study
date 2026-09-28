# 28-5. ASan report 읽기와 탐지 한계
## 1. 학습 목표
- ASan report에서 error category, access, source location, allocation context를 찾는다.
- report wording과 address를 고정된 C17 결과로 해석하지 않는다.
- ASan 탐지 범위와 dynamic analysis의 한계를 설명한다.
## 2. 선수 지식
28-4의 ASan build, Part 27의 array bounds와 object lifetime을 안다.
## 3. 핵심 개념
ASan report를 다음 순서로 읽는다.

1. memory-error category
2. read/write와 access size
3. 첫 user source location
4. 관련 allocation/deallocation context
5. summary

**[Sanitizer]** report는 instrumented execution에서 실제로 실행된 path의 지원되는 check 결과다. 실행되지 않은 path, 지원되지 않는 오류, instrumentation 밖 코드를 모두 증명하지 않는다.
## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -g -Og \
    -fsanitize=address asan_oob.c -o asan_oob
./asan_oob
```

report에서 `heap-buffer-overflow` 같은 category가 관찰될 수 있지만 exact 문구·frame 번호·address는 ASan/runtime version과 environment에 의존한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *values = malloc(2 * sizeof *values);

    if (values == NULL) {
        return 1;
    }
    values[0] = 10;
    values[1] = 20;
    printf("%d\n", values[1]);
    free(values);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o asan_report_defined
./asan_report_defined
```
## 6. 코드 해석
정상 최소 예제는 마지막 유효 원소인 `values[1]`을 읽어 `20`을 출력한다. invalid ASan 사례와 normal PASS count를 섞지 않는다.
## 7. 내부 동작
**[C17]** bounds 밖 access의 semantic category를 정한다.

**[Sanitizer]** shadow metadata와 inserted checks로 access를 검사하고 tool-specific report를 만든다.

**[OS / ABI]** process address, ASLR, signal, symbolization 결과가 report 모양에 영향을 줄 수 있다.

ASan clean은 “이 instrumented run에서 보고된 오류가 없다”는 관찰이지 모든 possible execution의 memory safety proof가 아니다. UBSan도 별도 instrumentation tool이며 이 curriculum Step의 ASan과 같은 완전한 ISO C validator가 아니다.
## 8. 자주 하는 실수
- 첫 address만 보고 source location을 읽지 않는다.
- library frame만 고치고 최초 user frame을 놓친다.
- ASan이 모든 memory bug를 검출한다고 한다.
- ASan clean을 UB-free proof로 사용한다.
- diagnostic text를 C17이 요구하는 output이라고 한다.
## 9. 필수 실습
28-4의 임시 OOB report에서 category와 첫 user source location을 찾고, index bound를 고친 뒤 재실행한다.
[28-5 exercise](../../exercises/28-debugging/28-5/README.md)
## 10. 추가 실습
- ★ read/write 종류를 표시한다.
- ★★ allocation context와 fault context를 분리한다.
- ★★★ unexecuted path가 clean run에 남기는 한계를 설명한다.
## 11. 확인 문제
1. report에서 먼저 찾을 항목은?
2. exact address를 예상값으로 고정하면 안 되는 이유는?
3. ASan clean이 증명하지 못하는 것은?
4. ASan과 C17 UB 분류의 관계는?
5. 최초 user source frame이 중요한 이유는?
## 12. 핵심 정리
- category보다 source-level 원인과 violated contract까지 추적한다.
- report와 종료 방식은 sanitizer/runtime 관찰이다.
- 수정 뒤 같은 입력과 normal tests를 모두 재실행한다.
## 13. 다음 Step
[28-6. GDB 실행과 breakpoint](28-6-running-gdb-and-breakpoints.md)
## 14. 참고 자료
- [GCC: Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
- [AddressSanitizer documentation](https://github.com/google/sanitizers/wiki/AddressSanitizer)
