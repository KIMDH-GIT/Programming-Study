# 14-4. 역참조 연산자 `*`

expression의 unary `*`는 유효한 pointer가 가리키는 object를 지정하는 indirection operator다.

## 1. 학습 목표
- declaration `*`와 expression `*`를 구분한다.
- `*pointer`로 pointed-to value를 읽는다.
- dereference의 유효성 전제를 설명한다.

## 2. 선수 지식
Step 14-2의 pointer value와 Step 14-3의 pointer declaration을 안다.

## 3. 핵심 개념
`int *pointer`의 `*`는 declarator 일부지만 `*pointer`의 `*`는 unary indirection operator다. pointer가 살아 있는 `int` object를 올바르게 가리킬 때 `*pointer`는 그 object를 지정하는 lvalue이고 읽으면 stored int value를 얻는다.

## 4. 문법
```c
int number = 10;
int *pointer = &number;
int copy = *pointer;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;
    int *pointer = &number;

    printf("number: %d\n", number);
    printf("*pointer: %d\n", *pointer);
    return 0;
}
```

## 6. 코드 해석
`pointer`는 `number`를 가리키는 pointer value를 저장한다. `*pointer`는 같은 `number` object를 지정하므로 두 출력 모두 10이다.

```text
pointer object                    number object
+----------------+ points to     +------+
| pointer value  | ------------> |  10  |
+----------------+               +------+
                                      ^
                                      |
                                *pointer designates
```

## 7. 내부 동작
[C17 abstract machine] unary `*` operand가 pointer to object/function이면 designated entity를 나타낸다. invalid value를 dereference하면 behavior가 정의되지 않을 수 있다. [implementation] compiler는 실제 load를 사용할 수 있지만 최적화로 register value를 직접 쓸 수도 있어 C expression과 instruction은 1:1이 아니다.

## 8. 자주 하는 실수
- declaration `*`와 indirection `*`를 같은 문법 역할로 설명한다.
- pointer value 자체를 `%d`로 출력한다.
- 초기화하지 않은 pointer를 dereference한다.
- `NULL`을 dereference하면 항상 특정 signal이 난다고 단정한다.

## 9. 필수 실습
int object를 가리키는 pointer를 만들고 object와 dereference 값을 비교한다. [실습 README](../../exercises/14-pointers/14-4/README.md)

## 10. 추가 실습
- ★ double pointer value 읽기
- ★★ 두 pointers가 같은 object를 읽기
- ★★★ declaration/expression `*` 분류 문제

## 11. 확인 문제
1. `int *p`의 `*` 역할은?
2. expression `*p`의 역할은?
3. `*p`의 result type은?
4. dereference 전에 필요한 전제는?
5. `*p`가 항상 CPU load 하나인가?

## 12. 핵심 정리
declarator `*`는 pointer type을 만들고 expression `*`는 valid pointer가 가리키는 typed object를 지정한다.

## 13. 다음 Step
[Step 14-5. 포인터를 통한 값 수정](14-5-modify-through-pointer.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.2
- [cppreference: Indirection operator](https://en.cppreference.com/w/c/language/operator_member_access.html#Dereference)
