# 26-8. `%p`로 주소 관찰
## 1. 학습 목표
- object pointers를 `%p`로 올바르게 출력한다.
- address observations를 portable layout 규칙으로 해석하지 않는다.
- function pointers와 object pointers를 구분한다.
## 2. 선수 지식
26-2 virtual address space와 Part 5 `%p`를 안다.
## 3. 핵심 개념
`printf("%p", (void *)&object)`로 object pointer representation을 관찰할 수 있다. exact format, numeric value, relative order는 implementation·build·execution 결과다.
## 4. 문법
```c
printf("%p\n", (void *)&object);
printf("%p\n", (void *)allocated);
```

function pointer를 `(void *)`로 바꾸어 `%p`로 출력하는 것은 portable C17 예제가 아니다.
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
    printf("%p\n", (void *)&static_value);
    printf("%p\n", (void *)&automatic_value);
    printf("%p\n", (void *)allocated);
    puts("observed 3 object addresses");
    free(allocated);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o address_app
./address_app
```
## 6. 코드 해석
세 object addresses를 출력하지만 PASS 조건은 program 성공과 마지막 sentinel line이다. 주소 문자열을 exact-match하지 않는다.
## 7. 내부 동작
**[C17]** object pointer와 `void *` conversion, `%p` contract를 제공한다.

**[compiler / linker]** actual storage와 relocations를 구현한다.

**[OS / executable format]** PIE, loader, ASLR이 observed addresses에 영향을 줄 수 있다.

**[CPU / ISA]** addresses를 사용하지만 C pointer semantics를 raw physical address로 정의하지 않는다.
## 8. 자주 하는 실수
- `%p`에 `int *`를 conversion 없이 전달한다.
- exact address를 expected output으로 고정한다.
- unrelated addresses의 `<`·`>` ordering을 일반화한다.
- function pointer를 object pointer 출력 규칙으로 다룬다.
- address 차이를 object distance로 해석한다.
## 9. 필수 실습
세 valid object pointers를 출력하고 fixed address assertion 없이 실행한다.
[26-8 exercise](../../exercises/26-memory-structure/26-8/README.md)
## 10. 추가 실습
- ★ 두 번 실행한다.
- ★★ optimization·PIE·ASLR 영향을 분류한다.
- ★★★ symbol address 관찰은 `nm`로 별도 조사한다.
## 11. 확인 문제
1. `%p`에 필요한 argument type은?
2. exact address를 고정하면 안 되는 이유는?
3. unrelated object address ordering은 portable한가?
4. function pointer를 같은 방식으로 출력할 수 있는가?
5. pointer는 physical address와 같은가?
## 12. 핵심 정리
- object pointer는 `(void *)`로 `%p`에 전달한다.
- values와 ordering은 observations다.
- function pointer 관찰은 portable object-pointer 예제와 분리한다.
## 13. 다음 Step
[26-9. OS·ABI·최적화·ASLR에 따른 차이](26-9-os-abi-optimization-and-aslr.md)
## 14. 참고 자료
- N1570 6.3.2.3, 7.21.6.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf)
- Linux man-pages: `proc_pid_maps(5)` — Linux-specific
