# 28-11. 결함 재현·수정·재검증
## 1. 학습 목표
- 재현 가능한 최소 사례에서 expected와 actual을 비교한다.
- evidence로 원인 가설 하나를 검증한 뒤 수정한다.
- 같은 재현 입력과 경계 입력으로 regression을 확인한다.
## 2. 선수 지식
28-1부터 28-10까지의 compiler·ASan·GDB 도구를 안다.
## 3. 핵심 개념
debugging은 “이것저것 바꿔서 되면 끝”이 아니다.

1. 문제를 재현한다.
2. expected와 actual을 기록한다.
3. source와 input을 최소화한다.
4. compiler diagnostics를 확인한다.
5. GDB·logging·sanitizer 중 맞는 도구로 state를 관찰한다.
6. 원인 가설을 하나 세운다.
7. evidence로 가설을 확인한다.
8. root cause를 수정한다.
9. 같은 입력과 경계 사례를 재검증한다.

defined logic bug는 program이 표준 규칙대로 실행되어도 요구사항과 다른 결과를 만드는 결함이다. UB와 구분한다.
## 4. 문법
⚠ 수정 전 defined logic bug — 실행해 재현할 수 있다.
```c
static int sum(const int values[], size_t count)
{
    int result = 0;

    for (size_t i = 0; i + 1 < count; ++i) {
        result += values[i]; /* 마지막 원소 누락 */
    }
    return result;
}
```

입력 `{1, 2, 3}`에서 expected 6, actual 3으로 안정적으로 재현된다. array bounds는 지키므로 이 사례는 UB가 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int sum(const int values[], size_t count)
{
    int result = 0;

    for (size_t i = 0; i < count; ++i) {
        result += values[i];
    }
    return result;
}

int main(void)
{
    int values[] = {1, 2, 3};

    printf("%d\n", sum(values, sizeof values / sizeof values[0]));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o fixed_sum
./fixed_sum
```
## 6. 코드 해석
loop condition을 실제 count 전체로 고쳐 세 원소를 모두 합한다. 재현 입력의 output은 expected와 같은 `6`이다.
## 7. 내부 동작
**[C17]** 수정 전·후 모두 유효한 bounds에서 defined하게 실행된다.

**[GCC diagnostics]** 이 logic bug는 warning이 없을 수 있다. warning 부재가 requirement 만족을 증명하지 않는다.

**[GDB / logging]** iteration별 `i`와 `result`를 관찰해 마지막 index가 실행되지 않는 가설을 확인할 수 있다. debug print는 output과 timing을 바꿀 수 있다.

**[tests]** 재현 입력, 빈 range, 원소 하나, 일반 range로 regression을 확인한다.
## 8. 자주 하는 실수
- 재현 없이 source를 먼저 바꾼다.
- 여러 가설을 동시에 수정해 원인을 잃는다.
- logic bug를 모두 UB라고 부른다.
- 한 성공 입력만 보고 수정이 끝났다고 한다.
- warning suppression이나 cast를 root fix로 사용한다.
## 9. 필수 실습
누락 bug를 재현하고 GDB로 마지막 실행 index를 확인한 뒤 condition 하나만 수정하고 재검증한다.
[28-11 exercise](../../exercises/28-debugging/28-11/README.md)
## 10. 추가 실습
- ★ 원소 하나인 input을 추가한다.
- ★★ empty range와 general range를 검증한다.
- ★★★ 재현·가설·evidence·수정 ledger를 작성한다.
## 11. 확인 문제
1. 재현을 수정 전에 해야 하는 이유는?
2. logic bug와 UB의 차이는?
3. minimal reproducer의 장점은?
4. 한 가설씩 검증해야 하는 이유는?
5. regression 재검증에는 무엇을 포함하는가?
## 12. 핵심 정리
- expected·actual·input을 고정해 결함을 재현한다.
- tool observation으로 하나의 root-cause 가설을 검증한다.
- 수정 뒤 같은 사례와 경계 사례를 다시 실행한다.
## 13. 다음 Step
[28-12. Part 28 종합 복습](28-12-part-28-review.md)
## 14. 참고 자료
- [GDB User Manual](https://sourceware.org/gdb/current/onlinedocs/gdb.html/)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
- [GCC: Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
