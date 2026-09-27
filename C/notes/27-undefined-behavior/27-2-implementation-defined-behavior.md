# 27-2. implementation-defined behavior
## 1. 학습 목표
- implementation-defined behavior의 문서화 의무를 설명한다.
- UB 및 unspecified behavior와 구분한다.
- C17의 negative signed right shift를 정확히 분류한다.
## 2. 선수 지식
Part 2의 정수형, Part 3의 signed 표현, Part 21의 shift를 안다.
## 3. 핵심 개념
**[C17 — Implementation-defined]** 둘 이상의 가능성이 있고 구현이 선택하되, 그 선택을 문서화해야 하는 behavior다. 이는 “표준이 아무 요구도 하지 않는” UB와 다르다.

대표 사례는 음수 signed integer의 right shift 결과와 plain `char`의 signedness다. implementation documentation, compiler manual, predefined macros 등으로 선택을 확인한다.
## 4. 문법
```c
char plain = '\xff';
int shifted = -8 >> 1;
```
두 식의 구체 결과는 구현 선택과 문서에 의존한다. 특히 음수 signed right shift는 C17에서 UB가 아니다.
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    printf("plain char: %s\n", CHAR_MIN < 0 ? "signed" : "unsigned");
    printf("-8 >> 1: %d\n", -8 >> 1);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o impl_defined
./impl_defined
```
## 6. 코드 해석
프로그램은 현재 구현의 두 선택을 관찰한다. 특정 출력값을 모든 C17 implementation에 대한 예상 출력으로 고정하지 않는다.
## 7. 내부 동작
**[C17]** implementation-defined behavior는 선택의 문서화를 요구한다. `CHAR_MIN`으로 plain `char`의 범위를 관찰할 수 있다.

**[compiler]** target과 option에 따라 선택이 달라질 수 있으며 compiler 문서가 근거다.

**[CPU / ISA]** 특정 shift instruction이 어떤 bit를 채우는지는 구현 선택의 한 재료일 뿐 C 규칙 자체가 아니다.

**[MIPS — 수업 기준]** MIPS shift instruction semantics와 C signed shift 분류를 구분한다.

**[RISC-V — 병행 학습]** RISC-V instruction 결과만으로 모든 C implementation의 결과를 정하지 않는다.
## 8. 자주 하는 실수
- implementation-defined를 UB와 같은 뜻으로 쓴다.
- 현재 GCC 결과를 모든 compiler의 C17 결과로 일반화한다.
- negative signed right shift를 UB라고 한다.
- two's complement와 arithmetic shift를 C17의 유일한 모델로 가정한다.
## 9. 필수 실습
현재 compiler 문서와 실행 결과를 함께 기록하되, C17 보장과 구현 선택을 분리한다.
[27-2 exercise](../../exercises/27-undefined-behavior/27-2/README.md)
## 10. 추가 실습
- ★ `CHAR_MIN`, `CHAR_MAX`, `UCHAR_MAX`를 출력한다.
- ★★ 같은 source를 다른 target/compiler 문서와 비교한다.
- ★★★ implementation-defined 항목과 확인 방법 표를 만든다.
## 11. 확인 문제
1. implementation-defined behavior에서 구현의 의무는?
2. negative signed right shift의 C17 분류는?
3. plain `char`의 signedness를 어떻게 확인할 수 있는가?
4. implementation-defined와 unspecified behavior의 차이는?
5. CPU 결과가 곧 C17 보장이 아닌 이유는?
## 12. 핵심 정리
- implementation-defined behavior는 선택되며 문서화된다.
- 음수 signed right shift와 plain `char` signedness는 대표 사례다.
- 관찰 결과에는 compiler·target 정보를 붙인다.
## 13. 다음 Step
[27-3. unspecified behavior](27-3-unspecified-behavior.md)
## 14. 참고 자료
- N1570 3.4.1, 4p8, 5.2.4.2.1, 6.5.7p5. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C language behavior](https://en.cppreference.com/w/c/language/behavior)
