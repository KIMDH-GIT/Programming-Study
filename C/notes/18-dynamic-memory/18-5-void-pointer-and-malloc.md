# 18-5. `void *`와 `malloc`

`malloc`은 요청한 byte 수의 초기화되지 않은 storage를 할당하고 성공 시 적절히 정렬된 `void *`를 반환한다.

## 1. 학습 목표
- `malloc` prototype과 반환 contract를 설명한다.
- C에서 explicit cast 없이 object pointer로 변환한다.
- 값을 저장하기 전에 uninitialized storage를 읽지 않는다.

## 2. 선수 지식
Part 14의 pointer type과 Step 18-4의 크기 검사를 안다.

## 3. 핵심 개념
```c
void *malloc(size_t size);
```
실패하면 null pointer, 성공하면 fundamental alignment를 요구하는 object type에 적합한 pointer를 반환한다. storage의 초기 내용은 indeterminate이므로 먼저 값을 저장한다.

## 4. 문법
```c
int *p = malloc(sizeof *p);
```
C17에서 `void *`와 object pointer 사이 변환에 explicit cast는 필요 없다. `(int *)malloc(...)`이 C syntax error는 아니지만 기본 권장 패턴은 아니다.

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof *p);

    if (p == NULL) {
        return 1;
    }
    *p = 55;
    printf("%d\n", *p);
    free(p);
    return 0;
}
```

## 6. 코드 해석
`<stdlib.h>`가 declaration을 제공한다. cast 없이 pointer를 받고, failure 검사 뒤 저장하고 읽으며 마지막에 해제한다.

## 7. 내부 동작
`sizeof *p`는 unevaluated operand이므로 값을 읽기 위한 실제 dereference가 아니다. allocator가 `mmap`, `brk` 또는 다른 방법을 쓰는지는 C17 contract가 아니다.

## 8. 자주 하는 실수
- `<stdlib.h>` 없이 호출한다.
- allocation 직후 `*p`를 먼저 읽는다.
- C에서도 malloc cast가 필수라고 생각한다.
- `malloc`을 C++ `new`와 같은 규칙이라고 설명한다.

## 9. 필수 실습
한 `int`를 allocation하고 failure 검사 뒤 저장·출력·해제한다.
[18-5 exercise](../../exercises/18-dynamic-memory/18-5/README.md)

## 10. 추가 실습
- ★ `double` object를 allocation한다.
- ★★ `sizeof *p`와 `sizeof p`를 비교한다.
- ★★★ fundamental alignment 보장 범위를 조사한다.

## 11. 확인 문제
1. `malloc` 성공 storage의 초기 내용은 어떤가?
2. C17에서 malloc cast가 필요한가?
3. `<stdlib.h>`가 필요한 이유는?
4. `sizeof *p`가 uninitialized 값을 읽는가?
5. `malloc`이 어떤 OS API를 쓰는지 C17이 정하는가?

## 12. 핵심 정리
- `malloc`은 초기화되지 않은 allocated storage를 제공한다.
- failure 검사 후 저장하고 읽는다.
- C에서는 cast 없이 반환값을 object pointer에 저장한다.

## 13. 다음 Step
[18-6. `malloc(n * sizeof *ptr)`](18-6-malloc-count-sizeof.md)

## 14. 참고 자료
- N1570 6.3.2.3 Pointers; 7.22.3.4 `malloc`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
- [GCC: C dialect options](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html)
