# 27-12. Part 27 종합 복습
## 1. 학습 목표
- Part 27의 행동 범주와 대표 UB를 종합한다.
- source, compiler, runtime, OS, sanitizer, ISA 층을 분리한다.
- UB를 실행하기 전에 precondition 검사로 차단한다.
## 2. 선수 지식
27-1부터 27-11까지를 학습했다.
## 3. 핵심 개념
```text
[C17 semantic category]
defined / undefined / unspecified / implementation-defined / constraint

[compiler diagnostic]
syntax·constraint 진단, optional warning, translation acceptance

[optimizer behavior]
defined execution을 전제로 한 변환

[runtime / OS observation]
출력, exit status, signal, memory mapping

[sanitizer observation]
instrumented execution에서 탐지한 report

[CPU / ISA]
instruction result, exception, trap
```

표준의 UB 정의는 “해당 동작에 요구사항을 부과하지 않는다”이다. “항상 crash”, “랜덤 결과”, “compiler가 마음대로 망가뜨림”은 정의가 아니다.
## 4. 문법
위험 연산 앞에서 contract를 검사한다.
```c
if (index < count && divisor != 0 && pointer != NULL) {
    /* 각 연산의 추가 precondition도 확인한 뒤 수행 */
}
```

⚠ 분석용 — 실행하지 않는다.
```c
i = i++ + 1; /* i의 side effect와 다른 access가 unsequenced: UB */
```
argument evaluation order가 unspecified인 것과 unsequenced side effects는 구분한다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>

static int safe_average(const int values[], size_t count, int *result)
{
    long sum = 0;

    if (values == NULL || result == NULL || count == 0 ||
        count > (unsigned long)LONG_MAX) {
        return 0;
    }
    for (size_t i = 0; i < count; ++i) {
        if ((values[i] > 0 && sum > LONG_MAX - values[i]) ||
            (values[i] < 0 && sum < LONG_MIN - values[i])) {
            return 0;
        }
        sum += values[i];
    }
    *result = (int)(sum / (long)count);
    return 1;
}

int main(void)
{
    int values[] = {10, 20, 30};
    int average;

    if (safe_average(values, 3, &average)) {
        printf("%d\n", average);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part27_review
./part27_review
```
## 6. 코드 해석
null, empty range, array bounds, signed `long` addition을 검사한 뒤 계산한다. 세 값의 평균 `20`을 출력한다.
## 7. 내부 동작
**[C17]** signed overflow, array OOB, null dereference, expired lifetime access, invalid pointer arithmetic, unsequenced side effects, library precondition 위반은 각 normative rule로 판단한다. Annex J는 요약이며 해당 clause와 함께 확인한다.

**[compiler]** diagnostic 유무와 optimization 결과는 UB 분류 자체가 아니다.

**[sanitizer]** 안전한 작은 사례를 임시 환경에서 관찰할 수 있지만 report나 무보고는 C17의 결과·증명이 아니다.

**[OS]** SIGSEGV·SIGFPE와 정상 종료는 가능한 runtime observation이다.

**[MIPS — 수업 기준]** arithmetic trap과 address exception은 MIPS ISA 관찰이며 C UB 정의가 아니다.

**[RISC-V — 병행 학습]** instruction의 wrap·trap 특성과 C source permission을 분리한다.
## 8. 자주 하는 실수
- warning·sanitizer·정상 실행 하나로 정확성을 증명한다.
- signed와 unsigned arithmetic을 같은 규칙으로 본다.
- one-past 형성과 dereference를 혼동한다.
- pointer bit만 보고 lifetime과 array 관계를 무시한다.
- C23·C++ 규칙을 C17 설명에 섞는다.
- error recovery 수단으로 null dereference나 signal 처리를 사용한다.
## 9. 필수 실습
정상 예제의 모든 precondition을 목록화하고 Part 27 사례를 행동 범주와 관찰 층으로 분류한다.
[27-12 exercise](../../exercises/27-undefined-behavior/27-12/README.md)
## 10. 추가 실습
- ★ 각 Step의 defined counterpart를 한 줄로 요약한다.
- ★★ compiler warning과 C17 diagnostic requirement를 비교한다.
- ★★★ sanitizer가 놓칠 수 있는 unexecuted path를 설명한다.
## 11. 확인 문제
1. UB의 C17 정의는?
2. implementation-defined와 unspecified behavior의 차이는?
3. compiler acceptance가 정확성을 증명하지 못하는 이유는?
4. one-past pointer에서 허용되는 것과 금지되는 것은?
5. sanitizer report는 어느 층의 결과인가?
6. hardware wrap이 C signed overflow를 정의하지 않는 이유는?
7. `undefined reference`와 UB는 왜 다른가?
## 12. 핵심 정리
- 행동 범주와 diagnostic requirement를 먼저 분류한다.
- UB 결과를 예측하지 말고 실행 전에 원인을 제거한다.
- C17, compiler, sanitizer, OS, ISA 설명을 명확히 라벨링한다.
## 13. 다음 Step
Part 28의 첫 Step은 **28-1. `-std=c17 -Wall -Wextra -Wpedantic`**이다. 이번 Part에서는 Part 28 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 3.4, 4, 5.1.1.3, 6.2.4, 6.5, 7.1.1, Annex J. N1570은 **C11 공개 Committee Draft**이며 인용한 규칙은 C17에서도 유지된다.
- [cppreference: C language behavior](https://en.cppreference.com/w/c/language/behavior)
- GCC manuals — diagnostics, optimization, instrumentation에 대한 implementation 자료
