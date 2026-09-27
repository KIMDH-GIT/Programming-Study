# 27-10. compiler optimization과 UB
## 1. 학습 목표
- optimizer가 defined C program을 전제로 추론하는 이유를 설명한다.
- C17 semantics와 특정 GCC generated code를 구분한다.
- `-O0`, `-O2`, sanitizer의 역할과 한계를 안다.
## 2. 선수 지식
27-1의 UB 정의와 27-4의 signed overflow를 안다.
## 3. 핵심 개념
compiler는 표준 규칙을 지키는 execution의 observable behavior를 보존하도록 프로그램을 변환한다. 따라서 defined execution에서는 signed overflow가 없고 pointer precondition이 지켜진다는 사실을 추론에 사용할 수 있다.

⚠ 분석용 — 실행하지 않는다.
```c
int greater_after_increment(int x)
{
    return x + 1 > x;
}
```
`x == INT_MAX` path는 `x + 1`에서 UB다. defined execution만 보면 비교는 참이므로 optimizer가 함수를 상수 결과로 단순화할 수 있다. 이는 compiler가 UB를 발견해 일부러 program을 망가뜨리는 행동이 아니다.
## 4. 문법
범위를 먼저 좁혀 모든 실행을 defined로 만든다.
```c
if (x == INT_MAX) {
    return 0;
}
return x + 1 > x;
```
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>

static int can_increment(int value)
{
    return value < INT_MAX;
}

int main(void)
{
    int value = 41;

    if (can_increment(value)) {
        ++value;
    }
    printf("%d\n", value);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -O2 main.c -o optimized
./optimized
```
## 6. 코드 해석
증가 전에 `INT_MAX`보다 작은지 검사한다. 출력은 optimization level과 무관하게 `42`다.
## 7. 내부 동작
**[C17 semantic category]** signed overflow가 있는 execution에는 표준 요구사항이 없다. C17은 특정 assembly를 요구하지 않는다.

**[GCC observation]** `-O0`과 `-O2`는 GCC options이며 generated code 차이는 현재 GCC version·target의 관찰이다.

**[sanitizer observation]** `-fsanitize=undefined`와 `-fsanitize=address`는 compiler/runtime instrumentation이다. report는 표준 결과가 아니며 clean run도 모든 possible execution이 UB-free임을 증명하지 않는다.

**[CPU / ISA]** hardware instruction이 wrap해도 source-level signed overflow를 허용하지 않는다.
## 8. 자주 하는 실수
- optimizer가 UB를 만나면 “마음대로” 악의적으로 동작한다고 한다.
- `-O0`이면 UB가 정의된다고 생각한다.
- current assembly를 C17의 필수 번역으로 설명한다.
- sanitizer 무보고를 완전한 안전성 증명으로 삼는다.
## 9. 필수 실습
defined example을 `-O0`과 `-O2`로 각각 build하고 같은 observable output을 확인한다.
[27-10 exercise](../../exercises/27-undefined-behavior/27-10/README.md)
## 10. 추가 실습
- ★ 두 optimization level의 binary size를 관찰한다.
- ★★ `gcc -S` 결과를 현재 toolchain 관찰로만 비교한다.
- ★★★ range check 전후의 control flow를 설명한다.
## 11. 확인 문제
1. optimizer는 어떤 execution을 보존해야 하는가?
2. signed-overflow example이 단순화될 수 있는 이유는?
3. `-O0`이 UB를 defined로 바꾸는가?
4. generated assembly가 C17 보장인가?
5. sanitizer clean 결과가 증명하지 못하는 것은?
## 12. 핵심 정리
- optimizer는 defined C execution을 전제로 유효한 추론을 한다.
- optimization options와 assembly는 implementation 관찰이다.
- sanitizer는 유용한 탐지 도구이지 ISO C 의미나 완전한 증명이 아니다.
## 13. 다음 Step
[27-11. compile 성공과 프로그램 정확성의 차이](27-11-compile-success-vs-correctness.md)
## 14. 참고 자료
- N1570 3.4.3, 5.1.2.3, 6.5p5. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Optimize Options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
- [GCC: Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
