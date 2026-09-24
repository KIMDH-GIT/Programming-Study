# 26-9. OS·ABI·최적화·ASLR에 따른 차이
## 1. 학습 목표
- 동일 source의 layout이 build·platform에 따라 달라질 수 있음을 설명한다.
- compiler optimization, ABI, PIE, ASLR을 C17과 구분한다.
- architecture labels를 섞지 않는다.
## 2. 선수 지식
26-2부터 26-8까지를 안다.
## 3. 핵심 개념
C observable semantics를 지키는 범위에서 compiler는 object representation을 바꿀 수 있다. ABI는 calling convention과 alignment 등을, linker와 executable format은 placement를, OS loader와 ASLR은 mappings를 결정할 수 있다.

한 실행의 memory map은 universal diagram이 아니다.
## 4. 문법
```c
static const int fixed_value = 5;
```

`fixed_value`가 반드시 별도 memory symbol이나 `.rodata` bytes로 남는다는 C17 보장은 없다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static const int fixed_value = 5;

int main(void)
{
    int automatic_value = fixed_value + 1;

    printf("value: %d\n", automatic_value);
    printf("address: %p\n", (void *)&automatic_value);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o environment_app
./environment_app
./environment_app
```
## 6. 코드 해석
portable result는 두 실행 모두 `value: 6`이다. observed address는 같거나 다를 수 있으며 PASS 조건으로 고정하지 않는다.
## 7. 내부 동작
**[C17]** required observable behavior와 object semantics를 정한다.

**[compiler / linker]** optimization, symbol elimination, PIE, section placement를 구현한다.

**[OS / executable format]** loader와 ASLR이 virtual mappings를 정할 수 있다. ASLR을 끄지 않는다.

**[CPU / ISA]** generated instructions와 ABI conventions를 따른다.

**[MIPS — 수업 기준]** `$sp`, `$ra` 같은 register 사용은 ABI와 compiler output 관찰이다.

**[RISC-V — 병행 학습]** `sp`, `ra` 역시 ABI convention이며 C17 layout rule이 아니다.
## 8. 자주 하는 실수
- optimization이 C object lifetime semantics를 임의로 깨뜨린다고 말한다.
- `const` object가 반드시 ROM·rodata에 있다고 말한다.
- ASLR을 C standard requirement라고 말한다.
- 단순화를 위해 PIE나 ASLR을 강제로 끈다.
- MIPS와 RISC-V register names를 섞는다.
## 9. 필수 실습
program을 두 번 실행해 deterministic value와 variable address observation을 분리한다.
[26-9 exercise](../../exercises/26-memory-structure/26-9/README.md)
## 10. 추가 실습
- ★ 두 실행의 value만 비교한다.
- ★★ default와 optimized build를 비교하되 semantics를 기준으로 판단한다.
- ★★★ ABI·linker·loader 책임을 표로 만든다.
## 11. 확인 문제
1. optimizer가 storage representation을 바꿀 수 있는 조건은?
2. ABI와 C17은 어떤 역할이 다른가?
3. ASLR은 어느 층인가?
4. const가 ROM placement를 보장하는가?
5. address variability가 program logic failure인가?
## 12. 핵심 정리
- C semantics와 physical representation을 분리한다.
- build·ABI·OS가 observations에 영향을 준다.
- deterministic behavior만 portable PASS 조건으로 사용한다.
## 13. 다음 Step
[26-10. Part 26 종합 복습](26-10-part-26-review.md)
## 14. 참고 자료
- N1570 5.1.2.3, 6.2.4. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- GCC optimization documentation
- System V ABI and Linux ASLR documentation — implementation references
