# 14-3. 포인터 변수 선언

pointer declaration은 pointer object가 어떤 type의 object를 가리킬 수 있는지 type system에 표현한다.

## 1. 학습 목표
- `int *p`, `double *q`, `char *c`를 읽는다.
- declarator의 `*` 역할을 설명한다.
- `int *p, q;`에서 두 identifiers의 type을 구분한다.

## 2. 선수 지식
Part 2의 declaration과 Step 14-2의 typed pointer value를 안다.

## 3. 핵심 개념
`int *p;`는 “p는 pointer to int type의 object”라고 읽는다. `*`는 이 declaration에서 declarator의 일부다. declaration은 각 declarator에 개별 적용되므로 `int *p, q;`에서 p만 `int *`이고 q는 `int`다. 초보 단계에서는 한 declaration에 identifier 하나를 쓰면 혼동을 줄일 수 있다.

## 4. 문법
```c
int *int_pointer;
double *double_pointer;
char *char_pointer;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;
    double ratio = 2.5;
    int *number_pointer = &number;
    double *ratio_pointer = &ratio;

    printf("%p\n", (void *)number_pointer);
    printf("%p\n", (void *)ratio_pointer);
    return 0;
}
```

## 6. 코드 해석
`number_pointer`의 type은 `int *`, `ratio_pointer`의 type은 `double *`다. 두 pointer types는 pointed-to object type 정보를 유지하며 각각 올바른 address를 저장한다.

## 7. 내부 동작
[C17 abstract machine] pointer types are derived types이며 referenced type이 type 관계를 결정한다. [ABI] 서로 다른 object pointer types의 실제 크기가 같은 구현이 흔하지만 모든 pointer type이 항상 같은 size나 representation이라는 보장은 이 예제에서 가정하지 않는다.

## 8. 자주 하는 실수
- `int *p, q;`의 q도 pointer라고 생각한다.
- declaration `*`를 dereference operator라고만 부른다.
- pointer type 차이는 address 크기와 무관하므로 중요하지 않다고 생각한다.
- pointer는 무조건 8 C bytes라고 말한다.

## 9. 필수 실습
int, double, char objects와 각각 맞는 pointer declarations를 작성한다. [실습 README](../../exercises/14-pointers/14-3/README.md)

## 10. 추가 실습
- ★ `int *p, q;` types 표시
- ★★ pointer와 pointed object의 `sizeof` 비교
- ★★★ 세 pointer types의 표준 보장/관찰 결과 구분

## 11. 확인 문제
1. `int *p`에서 p의 type은?
2. declaration의 `*` 역할은?
3. `int *p, q;`에서 q의 type은?
4. pointer type이 중요한 이유는?
5. 모든 pointer가 반드시 8 bytes인가?

## 12. 핵심 정리
pointer declarator는 pointed-to type을 포함하며 각 identifier의 declarator를 따로 읽어야 한다.

## 13. 다음 Step
[Step 14-4. 역참조 연산자 `*`](14-4-indirection-operator.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.7.6
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
