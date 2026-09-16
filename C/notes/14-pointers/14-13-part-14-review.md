# 14-13. Part 14 종합 복습

Part 14에서는 object address, typed pointer object, address-of, indirection, modification, NULL, lifetime과 invalid access를 학습했다.

## 1. 학습 목표
- pointer 핵심 objects·values·types를 종합한다.
- valid dereference 조건과 UB cases를 구분한다.
- C17 semantics와 ABI·OS·CPU 구현을 분리한다.

## 2. 선수 지식
Step 14-1부터 14-12까지와 Part 10의 pass-by-value·lifetime을 사용한다.

## 3. 핵심 개념
pointer는 다른 object를 가리키는 value를 저장할 수 있는 typed object다. `&object`는 pointer value를 만들고 `*pointer`는 valid target object를 지정한다. pointer representation은 단순 integer 물리 주소로 보장되지 않는다. NULL·uninitialized·dangling 상태를 구분하고 lifetime 안에서만 dereference한다.

## 4. 문법
```c
int value = 10;
int *pointer = &value;
*pointer = 20;
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int value = 10;
    int *pointer = &value;

    printf("pointer: %p\n", (void *)pointer);
    printf("before: %d\n", *pointer);
    *pointer = 20;
    printf("after: %d\n", value);

    pointer = NULL;
    if (pointer == NULL) {
        printf("no target\n");
    }
    return 0;
}
```

## 6. 코드 해석
pointer는 value object를 가리키고 10을 읽는다. indirection assignment 후 value는 20이다. 마지막에는 pointer에 null value를 저장하고 dereference하지 않는다.

## 7. 내부 동작
[C17 abstract machine] pointer type, conversion, address-of, indirection, null comparison, object lifetime 규칙이 동작을 정한다. [compiler/ABI] pointer size와 representation은 target implementation에 달린다. [OS] virtual address 사용은 흔하지만 필수가 아니다. [CPU] load/store 구현은 compiler output이며 C expression과 1:1이 아니다.

## 8. 자주 하는 실수
- pointer를 단순 integer physical address라고 부른다.
- 배열이나 string을 pointer와 같은 object라고 설명한다.
- pointer argument를 call by reference라고 부른다.
- NULL·uninitialized·dangling pointers를 dereference한다.
- pointer arithmetic, `void *`, const pointer, dynamic allocation을 이번 범위에 섞는다.

## 9. 필수 실습
valid pointer로 read·modify하고 NULL state로 전환하는 전체 흐름을 작성한다. [실습 README](../../exercises/14-pointers/14-13/README.md)

## 10. 추가 실습
- ★ expression type 표
- ★★ alias modification
- ★★★ C17·ABI·OS·CPU와 invalid states 점검표

## 11. 확인 문제
1. pointer object와 pointer value의 차이는?
2. declaration `*`와 indirection `*`의 차이는?
3. `&p`가 가리키는 object는?
4. valid dereference 조건은?
5. null dereference의 C17 분류는?
6. pointer가 항상 8 bytes인가?
7. pointer argument가 C의 pass-by-value를 바꾸는가?

## 12. 핵심 정리
pointer는 typed value를 저장하는 object이며 target type·lifetime·validity를 지켜 address-of와 indirection을 사용한다.

## 13. 다음 Step
Step 15-1. 배열 이름과 첫 원소 주소

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.4, 6.2.5, 6.3.2.3, 6.5.3.2, 6.7.6
- [cppreference: Pointers](https://en.cppreference.com/w/c/language/pointer.html)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
