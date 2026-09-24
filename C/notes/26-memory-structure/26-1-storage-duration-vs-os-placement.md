# 26-1. C storage duration과 OS 배치의 구분
## 1. 학습 목표
- storage duration을 C17 object semantics로 설명한다.
- scope, linkage, lifetime, storage duration을 구분한다.
- stack·heap·sections를 implementation 관찰로 분리한다.
## 2. 선수 지식
Part 18의 dynamic allocation과 Part 24의 scope·linkage를 안다.
## 3. 핵심 개념
C17은 object의 storage가 유지되는 기간을 storage duration으로 분류한다.

| declaration | scope | linkage | storage duration |
|---|---|---|---|
| file-scope `int global_value;` | file | external* | static |
| file-scope `static int hidden;` | file | internal | static |
| block `int local;` | block | none | automatic |
| block `static int saved;` | block | none | static |
| `malloc`으로 얻은 storage | identifier와 별개 | 해당 없음 | allocated |

`*` 정확한 linkage는 같은 identifier의 surrounding declarations도 고려해야 한다.

C17에는 thread storage duration도 있지만 이 Part에서는 `_Thread_local`을 새 주제로 확장하지 않는다.
## 4. 문법
```c
int global_value;

void update(void)
{
    int local_value = 10;
    static int persistent_value;
}
```

`global_value`와 `persistent_value`는 둘 다 static storage duration이지만 scope와 linkage는 다르다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int global_value = 10;

int next_value(void)
{
    int automatic_value = 1;
    static int persistent_value;

    ++persistent_value;
    return global_value + automatic_value + persistent_value;
}

int main(void)
{
    int *allocated = malloc(sizeof *allocated);

    if (allocated == NULL) {
        return 1;
    }
    *allocated = next_value();
    printf("%d %d\n", *allocated, next_value());
    free(allocated);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o duration_app
./duration_app
```
## 6. 코드 해석
`global_value`와 `persistent_value`는 static storage duration, `automatic_value`와 `allocated` pointer object는 automatic storage duration이다. `malloc`이 제공한 storage는 allocated storage duration을 가지며 `free`로 끝난다.
## 7. 내부 동작
**[C17]** storage duration, scope, linkage, lifetime은 서로 다른 규칙이다. static-duration objects는 program startup 전에 initialization되고, 명시적 initializer가 없는 경우 정해진 static initialization을 받는다.

**[compiler / linker]** 실제 storage를 register, instruction immediate, object-file section 등에 표현할 수 있다.

**[OS / executable format]** 전형적인 hosted implementation은 mappings와 allocator 영역을 사용하지만 C17이 stack·heap·ELF sections를 정의하지 않는다.

**[CPU / ISA]** 번역된 instructions가 필요할 때 address와 load/store를 사용한다. 특정 ISA가 C storage duration을 정하지 않는다.
## 8. 자주 하는 실수
- automatic storage duration을 stack이라고 정의한다.
- allocated storage duration을 heap storage duration이라고 부른다.
- scope를 object가 존재하는 시간이라고 설명한다.
- static keyword를 scope·linkage·duration 하나의 고정 의미로 본다.
- linker가 storage duration을 결정한다고 말한다.
## 9. 필수 실습
예제의 각 object를 scope·linkage·storage duration 표로 분류한다.
[26-1 exercise](../../exercises/26-memory-structure/26-1/README.md)
## 10. 추가 실습
- ★ `next_value`를 한 번 더 호출해 static local의 상태를 확인한다.
- ★★ file-scope static object를 표에 추가한다.
- ★★★ C17 semantics와 Linux 배치 관찰을 두 열로 분리한다.
## 11. 확인 문제
1. scope와 lifetime의 차이는?
2. block-scope static object의 linkage와 duration은?
3. automatic storage duration이 stack을 뜻하지 않는 이유는?
4. allocated storage duration이 heap과 같은 표준 용어인가?
5. initializer 유무가 storage duration 종류를 결정하는가?
## 12. 핵심 정리
- storage duration은 C object storage가 유지되는 기간의 분류다.
- scope, linkage, lifetime과 별도로 분석한다.
- 실제 section·mapping은 compiler·linker·OS 구현 영역이다.
## 13. 다음 Step
[26-2. process virtual address space](26-2-process-virtual-address-space.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.2, 6.2.4, 6.7.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Storage duration and linkage](https://en.cppreference.com/w/c/language/storage_duration)
- Linux man-pages: `proc_pid_maps(5)` — Linux-specific virtual mappings reference
