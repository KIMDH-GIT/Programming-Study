# 14-8. `NULL` pointer

null pointer value는 어떤 object나 function도 가리키지 않으며 dereference할 수 없다.

## 1. 학습 목표
- `NULL`을 null pointer constant macro로 사용한다.
- null 여부를 dereference 전에 검사한다.
- null pointer representation과 integer zero를 혼동하지 않는다.

## 2. 선수 지식
Step 14-4의 dereference validity와 Part 8의 conditions를 사용한다.

## 3. 핵심 개념
`int *pointer = NULL;`은 pointer를 어떤 object도 가리키지 않는 null pointer value로 초기화한다. integer constant expression 0도 null pointer constant로 사용할 수 있지만 null pointer의 실제 bit pattern이 all-bits-zero라는 보장은 아니다. `NULL` macro의 구체적 spelling도 모든 implementation에서 같다고 단정하지 않는다.

## 4. 문법
```c
#include <stddef.h>
int *pointer = NULL;
if (pointer != NULL) {
    value = *pointer;
}
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int number = 10;
    int *pointer = NULL;

    if (pointer == NULL) {
        printf("not pointing to an object\n");
    }

    pointer = &number;
    if (pointer != NULL) {
        printf("%d\n", *pointer);
    }
    return 0;
}
```

## 6. 코드 해석
처음 pointer는 null이라 dereference하지 않고 상태 문장만 출력한다. 이후 number address를 저장한 뒤 null이 아님을 확인해 10을 안전하게 읽는다.

## 7. 내부 동작
[C17 abstract machine] null pointer compares unequal to pointer to any object/function. null pointer dereference is Undefined Behavior. [implementation] NULL macro definition과 null representation은 implementation detail이다. 특정 OS에서 segmentation fault가 날 수 있지만 C17이 그 결과를 보장하지 않는다.

## 8. 자주 하는 실수
- NULL을 역참조해 어떤 signal이 나는지 실험한다.
- null pointer bit pattern이 항상 0이라고 단정한다.
- NULL macro가 항상 `((void *)0)`이라고 말한다.
- uninitialized pointer와 explicitly null pointer를 같은 상태로 본다.

## 9. 필수 실습
pointer를 NULL로 초기화하고 검사 후 valid object address를 저장해 안전하게 읽는다. [실습 README](../../exercises/14-pointers/14-8/README.md)

## 10. 추가 실습
- ★ null/non-null branch
- ★★ 사용 후 pointer를 NULL 상태로 돌리기
- ★★★ null constant·value·representation 구분표

## 11. 확인 문제
1. null pointer가 가리키는 object는?
2. NULL은 무엇인가?
3. null pointer bit pattern은 항상 all zero인가?
4. null dereference의 C17 분류는?
5. segmentation fault가 반드시 발생하는가?

## 12. 핵심 정리
NULL은 null pointer constant로 사용하며 null pointer는 dereference하지 않고 실제 representation을 integer zero와 동일시하지 않는다.

## 13. 다음 Step
[Step 14-9. 유효한 역참조 대상과 object lifetime](14-9-valid-dereference-lifetime.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.3, 6.5.3.2, 7.19
- [cppreference: NULL](https://en.cppreference.com/w/c/types/NULL.html)
