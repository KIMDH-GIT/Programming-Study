# 26-10. Part 26 종합 복습
## 1. 학습 목표
- storage duration·scope·linkage·lifetime을 종합한다.
- stack·heap·ELF·virtual memory를 implementation 층으로 분리한다.
- 주소와 tool output을 비이식적 관찰로 해석한다.
## 2. 선수 지식
26-1부터 26-9까지를 학습했다.
## 3. 핵심 개념
```text
[C17]
scope / linkage / storage duration / lifetime

[compiler / linker]
register allocation / symbols / sections / optimization

[OS / executable format]
virtual mappings / loader / ASLR / page protection

[CPU / ISA]
generated loads, stores, control transfer
```
## 4. 문법
```c
int file_object;
static int internal_object;

void f(void)
{
    int automatic_object;
    static int persistent_object;
}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

static int static_value;

static int next_value(void)
{
    int automatic_value = 1;
    static int saved;

    ++saved;
    return automatic_value + saved;
}

int main(void)
{
    int *allocated = malloc(sizeof *allocated);

    if (allocated == NULL) {
        return 1;
    }
    *allocated = next_value();
    printf("%d %d\n", static_value, *allocated);
    printf("allocated: %p\n", (void *)allocated);
    free(allocated);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o review_app
./review_app
```
## 6. 코드 해석
deterministic values는 `0 2`다. allocated address는 valid `%p` observation이지만 exact representation을 고정하지 않는다.
## 7. 내부 동작
**[C17]** static initialization, automatic and allocated durations, valid lifetime access를 규정한다.

**[compiler / linker]** actual storage와 symbols를 최적화·배치한다.

**[OS / executable format]** stack/allocator mappings, ELF layout, ASLR은 implementation이다.

**[CPU / ISA]** generated machine instructions를 실행한다.

**[MIPS — 수업 기준]** C properties와 MIPS ABI registers를 동일시하지 않는다.

**[RISC-V — 병행 학습]** C properties와 RISC-V ABI registers도 동일시하지 않는다.
## 8. 자주 하는 실수
- automatic = stack, allocated = heap이라고 정의한다.
- const = rodata/ROM이라고 정의한다.
- global = data, zero global = BSS라고 보장한다.
- pointer를 physical RAM address라고 말한다.
- scope, linkage, duration, lifetime을 같은 속성으로 본다.
- `.text/.data/.bss`를 C17 concepts라고 말한다.
## 9. 필수 실습
예제를 실행하고 모든 objects를 four properties와 implementation observation으로 분류한다.
[26-10 exercise](../../exercises/26-memory-structure/26-10/README.md)
## 10. 추가 실습
- ★ `next_value`를 두 번 호출한다.
- ★★ binutils 관찰과 C17 guarantees를 두 표로 분리한다.
- ★★★ hosted와 embedded implementation 사례를 비교한다.
## 11. 확인 문제
1. 네 C object properties는?
2. automatic과 stack의 차이는?
3. allocated duration과 heap의 차이는?
4. static zero initialization과 BSS의 관계는?
5. const와 OS read-only mapping의 차이는?
6. virtual address와 physical address의 차이는?
7. address observation을 고정하면 안 되는 이유는?
## 12. 핵심 정리
- C17 semantics를 먼저 설명하고 placement는 별도 관찰로 둔다.
- valid lifetime·allocation contracts를 지킨다.
- toolchain·OS·ABI·ISA 결과를 portable guarantee로 바꾸지 않는다.
## 13. 다음 Step
Part 27의 첫 Step은 **27-1. Undefined Behavior**이다. 이번 Part에서는 Part 27 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 6.2.1, 6.2.2, 6.2.4, 6.7.9, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C object and storage duration](https://en.cppreference.com/w/c/language/object)
- GNU binutils and Linux man-pages — implementation observation references
