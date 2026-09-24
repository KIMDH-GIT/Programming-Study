# 26-3. text 영역
## 1. 학습 목표
- function definition과 generated instructions의 관계를 설명한다.
- `.text`를 C17 개념이 아닌 toolchain 관찰로 구분한다.
- function pointer를 `%p`로 출력하지 않고 symbol tools를 사용한다.
## 2. 선수 지식
Part 10 function과 Part 24 compile·link 과정을 안다.
## 3. 핵심 개념
C17에서 function definition은 implementation이 executable behavior로 번역한다. C17은 `.text`라는 section 이름이나 executable format을 규정하지 않는다.

**[Linux + GCC/binutils 관찰]** ELF toolchain은 generated instructions와 관련 data를 `.text` 등 sections에 배치할 수 있다. ELF file section과 process memory mapping은 같은 개념이 아니다.
## 4. 문법
```c
static int message_length(void)
{
    return 5;
}
```

function pointer를 `(void *)`로 바꾸어 `%p`로 출력하는 것은 portable C17 주소 관찰 예제로 사용하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int message_length(void)
{
    return 5;
}

int main(void)
{
    printf("%d\n", message_length());
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o text_app
./text_app
```

**[Linux + GCC/binutils 관찰]**
```sh
nm text_app
readelf -S text_app
size text_app
```
symbol·section names와 sizes는 현재 build의 관찰값이며 exact value를 PASS 조건으로 고정하지 않는다.
## 6. 코드 해석
정상 C behavior는 `message_length()`가 5를 반환하는 것이다. function name이 symbol table에 남는지, 어느 section에 code가 배치되는지는 optimization·linking·format에 따라 달라질 수 있다.
## 7. 내부 동작
**[C17]** function call과 return behavior를 규정한다.

**[compiler / linker]** source function을 instructions로 번역하고 references를 연결한다. optimizer는 inline하거나 별도 symbol을 제거할 수 있다.

**[OS / executable format]** ELF sections와 loadable segments는 다른 structures다. loader는 program headers를 기준으로 mappings를 만들 수 있다.

**[CPU / ISA]** call과 control transfer는 target ISA·ABI에 맞는 instructions로 구현된다.

**[MIPS — 수업 기준]** `$ra`·`$sp` 사용은 ABI convention과 generated code의 문제이며 C17이 요구하지 않는다.

**[RISC-V — 병행 학습]** `ra`·`sp` register 사용도 ISA/ABI 구현이며 storage duration 규칙이 아니다.
## 8. 자주 하는 실수
- C17이 모든 function을 `.text`에 둔다고 말한다.
- `.text` file section과 executable memory page를 동일시한다.
- function pointer를 object pointer처럼 `%p`로 출력한다.
- 모든 function이 named linker symbol로 남는다고 생각한다.
- MIPS나 RISC-V ISA가 C function storage를 정의한다고 말한다.
## 9. 필수 실습
function을 호출하는 program을 실행하고 available binutils로 symbol·sections를 관찰한다.
[26-3 exercise](../../exercises/26-memory-structure/26-3/README.md)
## 10. 추가 실습
- ★ function 이름을 바꿔 `nm` 관찰을 비교한다.
- ★★ optimization build에서 symbol이 달라질 수 있는 이유를 설명한다.
- ★★★ ELF section과 segment 차이를 binutils 문서에서 조사한다.
## 11. 확인 문제
1. `.text`는 C17 storage duration 종류인가?
2. function behavior와 section placement의 차이는?
3. function pointer를 `%p`로 출력하지 않는 이유는?
4. optimizer가 named function symbol을 없앨 수 있는 이유는?
5. ELF section과 process mapping은 같은 개념인가?
## 12. 핵심 정리
- C17 function semantics와 generated code placement를 분리한다.
- `.text`는 특정 executable/toolchain 관찰 용어다.
- function 주소는 object-address `%p` 실습과 섞지 않는다.
## 13. 다음 Step
[26-4. initialized data와 BSS](26-4-initialized-data-and-bss.md)
## 14. 참고 자료
- N1570 6.9.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- GNU binutils documentation: `nm`, `readelf`, `size`
- System V ABI ELF specification — implementation format reference
