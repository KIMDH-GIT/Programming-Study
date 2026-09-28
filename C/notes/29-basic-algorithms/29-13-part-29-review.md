# 29-13. Part 29 종합 복습
## 1. 학습 목표
- Part 29의 contract·invariant·complexity를 종합한다.
- algorithm, C17, compiler, ISA 층을 분리한다.
- boundary-first validation으로 구현을 검토한다.
## 2. 선수 지식
29-1부터 29-12까지를 학습했다.
## 3. 핵심 개념
```text
[Algorithm]
problem contract / invariant / progress / termination / complexity

[C17]
array bounds / pointer validity / integer range / string precondition

[Compiler]
diagnostics / optimization / code generation

[CPU / ISA]
actual instructions / registers / cycles
```

correctness reasoning은 precondition 아래에서 postcondition이 성립하는 이유를 설명한다. deterministic tests와 sanitizers는 중요한 evidence지만 그 자체로 모든 input의 proof는 아니다.

실무에서는 검증된 library를 사용할 수 있지만 이번 Part는 algorithm의 진행과 경계 조건을 이해하려고 직접 구현한다. 먼저 correct·clear·boundary-safe한 ordinary C를 작성하고, macro-heavy generic code, XOR swap, branchless trick 같은 premature optimization은 도입하지 않는다.
## 4. 문법
Part 29 review 순서:

```text
1. input·output contract
2. empty·minimum·failure boundary
3. valid index range [0, count)
4. invariant와 progress
5. termination
6. postcondition
7. time·auxiliary-space complexity
8. C17 bounds·overflow·lifetime
9. deterministic tests
```
## 5. 최소 코드 예제
`contains_sorted`의 입력 contract는 `count > 0`일 때 `values`가 읽을 수 있는 `count`개 원소를 나타내고, 그 범위가 search comparison 기준 nondecreasing order로 정렬되어 있어야 한다는 것이다. `count == 0`이면 `values == NULL`도 허용한다. 정렬되지 않은 readable array는 이 algorithm의 precondition 위반이지만 그 사실만으로 C UB가 되는 것은 아니다.

```c
#include <stddef.h>
#include <stdio.h>

static int contains_sorted(const int values[], size_t count, int target)
{
    size_t begin = 0;
    size_t end = count;

    if (values == NULL && count != 0) {
        return 0;
    }
    while (begin < end) {
        size_t mid = begin + (end - begin) / 2;

        if (values[mid] < target) {
            begin = mid + 1;
        } else if (values[mid] > target) {
            end = mid;
        } else {
            return 1;
        }
    }
    return 0;
}

int main(void)
{
    int values[] = {1, 2, 4, 7};

    printf("found=%d\n", contains_sorted(values, 4, 4));
    printf("absent=%d\n", contains_sorted(values, 4, 3));
    printf("empty=%d\n", contains_sorted(NULL, 0, 3));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part29_review
./part29_review
```
## 6. 코드 해석
sorted precondition 아래 half-open range를 줄인다. target 4는 발견하고 3과 empty input은 미발견이다. 출력은 `found=1`, `absent=0`, `empty=0`이다.
## 7. 내부 동작
**[Algorithm]** candidate range invariant와 strict progress로 종료·correctness를 설명한다. Part 29의 sort, search, string, number algorithms도 같은 contract-first 관점으로 검토한다.

**[C17]** array bounds, null pointer, unsigned arithmetic, division by zero, string terminator를 각 algorithm contract와 별도로 확인한다.

**[Compiler]** warning 부재와 `-O2`는 correctness proof가 아니다. Big-O의 `O`는 compiler optimization option과 다른 개념이다.

**[MIPS — 수업 기준]** comparison·iteration 수는 MIPS instruction 수와 동일하지 않다.

**[RISC-V — 병행 학습]** branch나 register 관찰은 RISC-V ISA·ABI 층이며 C algorithm 정의가 아니다.
## 8. 자주 하는 실수
- normal input 하나로 boundary correctness를 대신한다.
- `size_t` 감소 loop에서 unsigned underflow를 종료 조건으로 쓴다.
- sorted precondition과 C UB를 같은 개념으로 본다.
- 모든 sort를 stable이라고 하거나 in-place를 변수 0개로 정의한다.
- ASan clean을 algorithm correctness proof로 사용한다.
## 9. 필수 실습
13개 Step의 contract, boundary, invariant, complexity, C17 safety를 한 표로 정리하고 대표 examples를 재검증한다.
[29-13 exercise](../../exercises/29-basic-algorithms/29-13/README.md)
## 10. 추가 실습
- ★ 각 Step의 empty input 동작을 요약한다.
- ★★ search와 sort의 invariant를 비교한다.
- ★★★ deterministic oracle과 implementation 결과를 임시 환경에서 비교한다.
## 11. 확인 문제
1. contract와 test case는 어떤 관계인가?
2. loop invariant와 termination은 각각 무엇을 설명하는가?
3. Big-O와 compiler `-O2`가 다른 이유는?
4. sanitizer clean이 correctness proof가 아닌 이유는?
5. half-open range가 empty input을 표현하는 방법은?
6. algorithm operation count와 ISA instruction count가 다른 이유는?
7. Part 29 구현에서 반복한 C17 safety 항목은?
## 12. 핵심 정리
- problem contract에서 구현과 tests를 도출한다.
- invariant·progress·termination으로 결과가 맞는 이유를 설명한다.
- algorithm, C17, compiler, ISA evidence의 범위를 구분한다.
## 13. 다음 Step
Part 30의 첫 Step은 **30-1. 자기 참조 구조체 `Node`**이다. 이번 Part에서는 Part 30 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 6.2.5, 6.5, 6.7.6.3, 7.24. N1570은 **C11 공개 Committee Draft**이며 인용한 규칙은 C17에서도 유지된다.
- NIST Dictionary of Algorithms and Data Structures.
- GCC manuals — diagnostics와 optimization에 대한 implementation 자료.
