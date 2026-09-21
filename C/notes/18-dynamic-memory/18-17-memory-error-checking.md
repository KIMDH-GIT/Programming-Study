# 18-17. 검사 실행 예제로 메모리 오류 관찰

compiler sanitizer는 C17의 일부가 아닌 구현 도구다. 이 Step에서는 정의된 동작의 프로그램을 instrumented build로 실행하고, UB 사례는 실행 없이 report 구조만 분석한다.

## 1. 학습 목표
- AddressSanitizer build와 일반 C17 의미를 구분한다.
- 정상 프로그램에서 report가 없음을 확인한다.
- use-after-free·out-of-bounds report 예시를 원인별로 읽는다.

## 2. 선수 지식
Step 18-14~16의 memory errors와 compiler option을 안다.

## 3. 핵심 개념
sanitizer는 많은 memory errors를 탐지할 수 있지만 모든 UB를 증명하거나 C17 의미를 바꾸지 않는다. 특정 report 형식과 탐지 범위는 GCC/Clang·platform 구현에 달렸다.

## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic \
    -fsanitize=address,undefined -g program.c -o program
./program
```

다음은 **가상의 report 발췌**다. 실제 주소와 문구는 도구·버전에 따라 달라진다.

```text
ERROR: AddressSanitizer: heap-use-after-free
READ of size 4 at example.c:14
freed by thread T0 here:
    example.c:13
previously allocated here:
    example.c:8
```

이 report는 14행의 access가 13행의 `free` 뒤에 일어났고, 대상 allocation은 8행에서 시작되었다는 관계를 보여 준다. 이 자료는 report 읽기용이며 UB 프로그램을 실행하라는 지시가 아니다.

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
    values[0] = 3;
    values[1] = 6;
    printf("%d\n", values[1]);
    free(values);
    return 0;
}
```

## 6. 코드 해석
allocation failure, bounds, initialization, deallocation을 모두 지킨다. sanitizer 실행에서 report가 없다는 관찰은 이 실행에서 탐지된 문제가 없다는 뜻이지 모든 correctness의 증명은 아니다.

## 7. 내부 동작
- **[C17]** UB 분류와 library contracts를 정한다.
- **[GCC/Clang]** instrumentation code와 runtime report를 추가한다.
- **[OS]** process·virtual address·signal 환경을 제공한다.

## 8. 자주 하는 실수
- sanitizer가 C17 기능이라고 생각한다.
- report가 없으면 모든 입력에서 완전히 안전하다고 단정한다.
- UB를 일부러 실행해야만 배울 수 있다고 생각한다.

## 9. 필수 실습
정상 예제를 sanitizer options로 build·run하고 report가 없음을 확인한다. 4절의 가상 use-after-free report에서 access·free·allocation 위치를 읽되 UB 코드는 실행하지 않는다.
[18-17 exercise](../../exercises/18-dynamic-memory/18-17/README.md)

## 10. 추가 실습
- ★ build flags의 역할을 구분한다.
- ★★ leak report와 UB report 차이를 조사한다.
- ★★★ sanitizer가 놓칠 수 있는 범주를 문서화한다.

## 11. 확인 문제
1. AddressSanitizer가 C17 표준 기능인가?
2. report가 없으면 무엇만 알 수 있는가?
3. sanitizer가 UB를 defined behavior로 바꾸는가?
4. report의 access와 allocation/free 위치는 왜 중요한가?

## 12. 핵심 정리
- sanitizer는 구현 도구다.
- 정의된 예제로 도구 사용법을 익힌다.
- UB 사례는 report와 source를 분석하고 실행하지 않는다.

## 13. 다음 Step
[18-18. 동적 배열 입력·평균·최댓값·최솟값](18-18-dynamic-array-statistics.md)

## 14. 참고 자료
- [GCC: Program instrumentation options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
- [Clang: AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html)
- N1570 3.4.3 Undefined behavior. N1570은 **C11 공개 Committee Draft**이며 관련 분류는 C17에서도 유지된다.
