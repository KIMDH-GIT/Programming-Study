# 26-6. heap과 동적 객체
## 1. 학습 목표
- allocated storage duration을 C17 용어로 설명한다.
- allocated storage와 구현의 heap을 구분한다.
- `malloc`·`free`와 lifetime을 연결한다.
## 2. 선수 지식
Part 18 dynamic memory와 26-1 storage duration을 안다.
## 3. 핵심 개념
`malloc`이 성공하면 요청 크기의 storage를 제공하고, 그 storage는 해제될 때까지 allocated storage duration을 가진다. C17의 duration 이름은 “heap storage duration”이 아니다.

일반 allocator는 process heap, anonymous mappings, caches 등을 사용할 수 있지만 항상 `brk`, `mmap`, 또는 호출당 syscall 하나를 사용한다고 일반화할 수 없다.
## 4. 문법
```c
int *value = malloc(sizeof *value);
if (value == NULL) {
    /* allocation failure */
}
free(value);
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *value = malloc(sizeof *value);

    if (value == NULL) {
        return 1;
    }
    *value = 26;
    printf("%d\n", *value);
    free(value);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o allocated_app
./allocated_app
```
## 6. 코드 해석
pointer object `value`는 automatic storage duration이고, 가리키는 allocated storage는 `free`까지 유지된다. `free` 뒤 그 storage를 접근하지 않는다.
## 7. 내부 동작
**[C17]** allocation functions의 contract와 allocated storage duration을 규정한다.

**[compiler / linker]** library call과 pointer operations를 번역한다.

**[OS / executable format]** allocator는 OS mappings나 process heap을 구현 전략으로 사용할 수 있다.

**[CPU / ISA]** returned pointer를 통한 valid load/store를 실행한다.
## 8. 자주 하는 실수
- C17의 공식 duration 이름을 heap이라고 말한다.
- `malloc`마다 syscall이 정확히 한 번 발생한다고 말한다.
- allocation failure를 검사하지 않는다.
- `free` 뒤 dereference를 실행해 관찰한다.
- memory leak을 새 storage-duration 종류라고 부른다.
## 9. 필수 실습
한 `int` storage를 할당·검사·사용·해제한다.
[26-6 exercise](../../exercises/26-memory-structure/26-6/README.md)
## 10. 추가 실습
- ★ 두 `int`를 위한 배열을 할당한다.
- ★★ pointer object와 allocated object의 durations를 비교한다.
- ★★★ allocator가 여러 OS 전략을 쓸 수 있는 이유를 조사한다.
## 11. 확인 문제
1. allocated storage duration과 heap은 같은 표준 용어인가?
2. pointer object와 allocated storage의 duration은?
3. `malloc`이 항상 syscall을 호출하는가?
4. `free` 뒤 pointer dereference가 잘못된 이유는?
5. leak은 duration category인가?
## 12. 핵심 정리
- C17 용어는 allocated storage duration이다.
- heap은 흔한 implementation 용어다.
- allocation failure와 lifetime 종료를 정확히 처리한다.
## 13. 다음 Step
[26-7. global·static·local·heap 객체 분석](26-7-object-category-analysis.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Dynamic memory management](https://en.cppreference.com/w/c/memory)
- Linux man-pages: `malloc(3)` — implementation interface reference
