# 28-4. AddressSanitizer build와 실행
## 1. 학습 목표
- AddressSanitizer build command를 작성한다.
- defined 정상 실행과 intentional invalid training run을 분리한다.
- ASan을 GCC runtime instrumentation tool로 설명한다.
## 2. 선수 지식
Part 18의 allocation·free, Part 27의 OOB·use-after-free, 28-3의 debug build를 안다.
## 3. 핵심 개념
**[Sanitizer / GCC]** `-fsanitize=address`는 memory access에 runtime checks를 추가해 out-of-bounds와 use-after-free 계열 오류를 찾도록 돕는다. ISO C17 기능이 아니며 모든 memory bug를 검출하지 않는다.

`-g`는 report의 source 정보를 유용하게 하고, `-Og`는 debugging-friendly optimization을 제공한다. 현재 GCC 문서는 sanitizer와 `-Werror` 조합에서 추가 warning 가능성을 지적하므로 sanitizer 관찰 command와 normal strict validation command를 구분한다.
## 4. 문법
정상 source build:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic \
    -g -Og -fsanitize=address main.c -o asan_app
```

⚠ ASan 격리 관찰용 UB — `/tmp/part28-validation/`에서만 실행한다.
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    size_t index;
    int *values;

    if (argc != 2 || argv[1][0] < '0' || argv[1][0] > '9' ||
        argv[1][1] != '\0') {
        return 2;
    }
    index = (size_t)(argv[1][0] - '0');
    values = malloc(2 * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    values[0] = 10;
    values[1] = 20;
    printf("%d\n", values[index]); /* argument 2: heap out-of-bounds */
    free(values);
    return 0;
}
```
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
    printf("%d\n", values[0] + values[1]);
    free(values);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic \
    -g -Og -fsanitize=address main.c -o asan_defined
./asan_defined
```
## 6. 코드 해석
정상 예제는 할당 성공을 확인하고 두 원소만 access한 뒤 한 번 해제한다. 출력은 `30`이며 ASan report가 없어야 한다.
## 7. 내부 동작
**[C17]** allocation contract, array bounds, lifetime을 규정한다.

**[GCC / Sanitizer]** access 주변에 checks를 instrument하고 runtime library를 연결한다.

**[Sanitizer observation]** invalid training source의 report text와 process exit는 ASan version·runtime 결과다.

**[OS]** address mappings와 signals는 host observation이며 C17이 요구하는 UB 결과가 아니다.
## 8. 자주 하는 실수
- ASan을 ISO C validator라고 부른다.
- ASan clean 한 번으로 모든 execution의 memory safety를 증명한다.
- report를 보기 위해 wild pointer write나 반복 heap corruption을 실행한다.
- sanitizer binary를 repo에 남긴다.
## 9. 필수 실습
defined source를 정상 실행한 뒤 별도 임시 파일에 argument `2`를 주어 작은 heap OOB를 ASan으로 한 번 관찰한다.
[28-4 exercise](../../exercises/28-debugging/28-4/README.md)
## 10. 추가 실습
- ★ build command의 각 option을 설명한다.
- ★★ valid index와 invalid index를 두 별도 실행으로 비교한다.
- ★★★ ASan instrumentation과 C17 semantics를 두 열로 정리한다.
## 11. 확인 문제
1. `-fsanitize=address`는 어느 층의 기능인가?
2. ASan이 주로 찾는 오류 범주는?
3. `-g`를 함께 쓰는 이유는?
4. ASan report가 C17의 UB 결과인가?
5. sanitizer artifact는 어디에 두는가?
## 12. 핵심 정리
- ASan은 instrumented execution에서 memory 오류를 찾는 dynamic tool이다.
- normal PASS와 expected ASan diagnostic을 별도로 보고한다.
- intentional invalid source와 binary는 임시 공간에만 둔다.
## 13. 다음 Step
[28-5. ASan report 읽기와 탐지 한계](28-5-reading-asan-reports-and-limitations.md)
## 14. 참고 자료
- [GCC: Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
- [AddressSanitizer documentation](https://github.com/google/sanitizers/wiki/AddressSanitizer)
