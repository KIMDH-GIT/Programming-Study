# 26-2. process virtual address space
## 1. 학습 목표
- process virtual address space를 OS 개념으로 설명한다.
- C pointer value와 physical RAM address를 구분한다.
- 주소 관찰을 portable ordering 규칙으로 바꾸지 않는다.
## 2. 선수 지식
26-1 storage duration과 Part 14 pointer를 안다.
## 3. 핵심 개념
현대 hosted OS는 각 process에 virtual address space라는 추상화를 제공할 수 있다. code/data mappings, allocator가 사용하는 mappings, stack mapping 등이 보일 수 있지만 이는 OS·executable format·ABI·build 설정의 결과다.

```text
전형적인 process virtual-address-space 예시
≠ ISO C17이 요구하는 layout
≠ physical RAM 배치도
```

C pointer value를 “물리 RAM 주소”라고 정의하지 않는다.
## 4. 문법
```c
printf("%p\n", (void *)&object);
```

`%p`에는 `void *` argument를 전달한다. 출력 representation과 numeric order는 implementation 관찰이다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

static int static_value;

int main(void)
{
    int automatic_value = 1;
    int *allocated = malloc(sizeof *allocated);

    if (allocated == NULL) {
        return 1;
    }
    *allocated = 2;
    printf("static:    %p\n", (void *)&static_value);
    printf("automatic: %p\n", (void *)&automatic_value);
    printf("allocated: %p\n", (void *)allocated);
    printf("sum: %d\n", static_value + automatic_value + *allocated);
    free(allocated);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o address_space_app
./address_space_app
```
## 6. 코드 해석
세 object address를 올바른 `%p` form으로 관찰한다. PASS 조건은 실행 성공과 `sum: 3`이며 특정 address나 순서를 고정하지 않는다.
## 7. 내부 동작
**[C17]** object pointer values, conversions to `void *`, `%p` interface를 규정하지만 virtual memory를 요구하지 않는다.

**[compiler / linker]** objects를 제거·병합하지 않는 범위와 build 설정에 따라 storage representation을 만든다.

**[OS / executable format]** loader가 executable과 libraries를 virtual mappings로 배치할 수 있다. ASLR은 OS security feature다.

**[CPU / ISA]** instructions가 virtual addresses를 사용하고 platform memory system이 이를 처리할 수 있다. C pointer를 physical address와 동일시하지 않는다.
## 8. 자주 하는 실수
- pointer에는 항상 physical RAM address가 들어 있다고 말한다.
- virtual address-space 그림을 physical memory 배치도라고 부른다.
- global < heap < stack 같은 portable address 순서를 만든다.
- stack은 항상 아래로, heap은 항상 위로 자란다고 일반화한다.
- 실행마다 address가 다르면 C program이 잘못됐다고 생각한다.
## 9. 필수 실습
세 object addresses와 deterministic sum을 출력하고 address 값은 PASS assertion에 사용하지 않는다.
[26-2 exercise](../../exercises/26-memory-structure/26-2/README.md)
## 10. 추가 실습
- ★ program을 두 번 실행해 주소가 달라질 수 있음을 관찰한다.
- ★★ virtual과 physical address 차이를 한 문단으로 설명한다.
- ★★★ Linux `/proc/<pid>/maps`가 보여주는 것이 C17 규칙이 아닌 이유를 조사한다.
## 11. 확인 문제
1. virtual address space는 C17 개념인가?
2. pointer를 physical address라고 정의할 수 없는 이유는?
3. `%p` argument에 `(void *)` conversion을 쓰는 이유는?
4. unrelated object addresses로 portable ordering을 만들 수 있는가?
5. ASLR은 어느 층의 기능인가?
## 12. 핵심 정리
- virtual address space는 OS implementation 개념이다.
- C pointer semantics와 physical addressing을 분리한다.
- 주소 관찰값은 현재 실행의 결과일 뿐 portable layout 규칙이 아니다.
## 13. 다음 Step
[26-3. text 영역](26-3-text-region.md)
## 14. 참고 자료
- N1570 6.2.4, 6.3.2.3, 7.21.6.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Pointer](https://en.cppreference.com/w/c/language/pointer)
- Linux man-pages: `proc_pid_maps(5)` and `mmap(2)` — Linux-specific
