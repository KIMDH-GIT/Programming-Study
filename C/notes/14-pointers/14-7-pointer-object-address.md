# 14-7. 포인터 자체의 주소

pointer variable도 object이므로 자신의 storage와 address를 가지며 `&pointer`로 그 pointer object를 가리킬 수 있다.

## 1. 학습 목표
- pointed-to object address와 pointer object address를 구분한다.
- `p`와 `&p`의 type 차이를 설명한다.
- pointer-to-pointer는 이번 Step의 최소 범위로만 다룬다.

## 2. 선수 지식
Step 14-6의 `p`와 `&p` 관계를 안다.

## 3. 핵심 개념
`p`는 number를 가리키는 `int *` value다. `&p`는 p object 자체를 가리키며 type은 `int **`다. 이 둘은 가리키는 대상과 type이 다르다. pointer-to-pointer의 선언·이중 역참조는 후속 범위로 남기고 이번에는 pointer object도 address를 가진다는 사실에 집중한다.

## 4. 문법
```c
int number = 10;
int *p = &number;
printf("%p\n", (void *)p);
printf("%p\n", (void *)&p);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;
    int *pointer = &number;

    printf("number address: %p\n", (void *)&number);
    printf("stored pointer: %p\n", (void *)pointer);
    printf("pointer object address: %p\n", (void *)&pointer);
    return 0;
}
```

## 6. 코드 해석
첫 두 outputs는 모두 number를 가리키는 pointer value라 같은 표현이다. 셋째는 pointer object `pointer` 자체의 address이므로 대상이 다르다.

## 7. 내부 동작
[C17 abstract machine] object pointer도 object이고 unary `&` operand가 될 수 있다. `&pointer`의 type level이 하나 더 깊어지지만 실제 pointer representations와 sizes는 implementation이 정한다. pointer object를 가리킨다고 pointed-to number를 소유하는 것은 아니다.

## 8. 자주 하는 실수
- pointer object에는 address가 없다고 생각한다.
- pointer와 pointer의 address를 같은 값이라고 생각한다.
- `&pointer`가 number를 가리킨다고 생각한다.
- 이 Step에서 이중 역참조나 complex pointer chains까지 확장한다.

## 9. 필수 실습
ordinary object address, 저장된 pointer value, pointer object address를 세 줄로 관찰한다. [실습 README](../../exercises/14-pointers/14-7/README.md)

## 10. 추가 실습
- ★ double pointer object address 관찰
- ★★ 두 pointer objects가 같은 number를 가리키게 하기
- ★★★ type level diagram 작성

## 11. 확인 문제
1. p의 type은?
2. `&p`의 type은?
3. p가 가리키는 object는?
4. `&p`가 가리키는 object는?
5. pointer size가 type level마다 항상 같은가?

## 12. 핵심 정리
pointer도 object이므로 자체 address가 있으며 stored pointer value와 pointer object address는 type과 대상이 다르다.

## 13. 다음 Step
[Step 14-8. `NULL` pointer](14-8-null-pointer.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.3.2.3
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
