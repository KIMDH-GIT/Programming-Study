# 31-1. C abstract machine과 observable behavior
## 1. 학습 목표
- C abstract machine을 실제 CPU나 runtime VM과 구분한다.
- observable behavior와 as-if transformation의 관계를 설명한다.
- source object와 executable의 memory slot/register를 동일시하지 않는다.
## 2. 선수 지식
Part 27의 undefined behavior, Part 30의 object lifetime, hosted C의 `printf` 사용을 안다.
## 3. 핵심 개념
**[C17 abstract machine]** C 표준은 source program의 의미를 추상적 실행 모델로 기술한다. 이는 JVM이나 VMware처럼 별도 runtime VM이 반드시 존재한다는 뜻이 아니다.

정의된 프로그램에서 implementation은 표준이 요구하는 observable behavior를 보존하는 범위에서 계산을 합치거나 제거하고 object를 register에 둘 수 있다. C 문장 하나, machine instruction 하나, C object 하나, 실제 memory slot 하나는 서로 1:1 대응하지 않는다.

N1570 5.1.2.3의 최소 요구에는 volatile object access, program 종료 시 file 내용, interactive device의 input/output 동작 등이 포함된다. `printf` 같은 library I/O의 요구되는 효과까지 아무 이유 없이 제거할 수 있다는 뜻은 아니다.
## 4. 문법
```c
static int transform(int value);
```
source-level function과 object는 C 의미를 표현한다. compiler가 이를 어떤 instruction과 register로 구현하는지는 별도 층이다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int transform(int value)
{
    int doubled = value * 2;
    return doubled + 1;
}

int main(void)
{
    printf("result=%d\n", transform(20));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o abstract_machine
./abstract_machine
```
## 6. 코드 해석
정의된 실행의 출력은 `result=41`이다. compiler는 `transform`을 호출하거나 inline할 수 있고 중간 object `doubled`에 별도 저장 공간을 만들지 않을 수도 있지만, 요구되는 출력은 보존해야 한다.
## 7. 내부 동작
**[compiler]** GCC 13.3.0의 `-O0`과 `-O2` 결과는 현재 compiler/target 관찰일 뿐 C17의 특정 optimization 요구가 아니다.

**[ABI / CPU]** calling convention, register 선택, instruction sequence는 ABI와 target code generation 문제다. C variable이 hardware register이거나 pointer가 physical RAM address라고 정의되지 않는다.

**[MIPS — 수업 기준]** assignment가 항상 `sw`, dereference가 항상 `lw` 하나라는 대응은 없다.

**[RISC-V — 병행 학습]** compiler는 같은 C 의미를 여러 instruction 또는 instruction 없이 구현할 수 있다.
## 8. 자주 하는 실수
- abstract machine을 실행 시 설치되는 가상머신으로 설명한다.
- C 한 줄이 machine instruction 하나라고 단정한다.
- observable effect가 없는 정상 계산의 제거와 UB 기반 변환을 혼동한다.
- `printf` output도 compiler가 항상 지울 수 있다고 일반화한다.
## 9. 필수 실습
strict build와 실행 후 `-O0 -S`, `-O2 -S` 결과에서 함수와 중간 object 표현이 달라질 수 있음을 관찰한다. [31-1 exercise](../../exercises/31-system-embedded-c/31-1/README.md)
## 10. 추가 실습
- ★ output 값을 바꾸고 C 의미가 유지되는지 확인한다.
- ★★ 두 optimization level의 assembly 줄 수만 비교한다.
- ★★★ 특정 assembly를 표준 보장으로 오해하면 안 되는 이유를 기록한다.
## 11. 확인 문제
1. C abstract machine은 runtime VM인가?
2. observable behavior를 보존한다는 말은 무엇인가?
3. C object가 executable의 별도 memory slot을 보장하지 않는 이유는?
4. source 한 줄과 machine instruction이 1:1이 아닌 이유는?
5. `printf`가 있는 예제에서 보존해야 할 것은 무엇인가?
## 12. 핵심 정리
- C 의미는 먼저 abstract machine 관점에서 정의된다.
- implementation은 요구되는 observable behavior 안에서 실행 방식을 바꾼다.
- source, compiler, ABI, CPU 층을 구분한다.
## 13. 다음 Step
[31-2. `volatile` access와 compiler optimization](31-2-volatile-access-and-compiler-optimization.md)
## 14. 참고 자료
- WG14 N1570 5.1.2.3. N1570은 C11 공개 Committee Draft이며 이 abstract-machine 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, Optimize Options: https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html
- GCC, C language standards and `-std=c17`: https://gcc.gnu.org/onlinedocs/gcc/Standards.html
