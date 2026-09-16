# 14-6. `a`, `&a`, `p`, `&p`, `*p`

같은 코드에서도 object value, object address, pointer value, pointer object address, pointed-to value는 서로 다른 expression이다.

## 1. 학습 목표
- 다섯 expressions의 type과 의미를 구분한다.
- `%d`와 `%p`를 expression type에 맞게 사용한다.
- pointer diagram으로 관계를 설명한다.

## 2. 선수 지식
Step 14-1~14-5의 address·declaration·indirection을 모두 사용한다.

## 3. 핵심 개념
`int a = 10; int *p = &a;`에서 `a`와 `*p`는 같은 int object의 value를 읽는다. `&a`와 `p`는 a를 가리키는 pointer values다. `&p`는 pointer object p 자체를 가리키는 또 다른 pointer value다. `&a`와 `&p`는 다른 objects를 가리킨다.

## 4. 문법
```c
a       /* int value */
&a      /* pointer to int */
p       /* pointer to int value */
&p      /* pointer to pointer to int */
*p      /* int object/value context */
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int a = 10;
    int *p = &a;

    printf("a: %d\n", a);
    printf("&a: %p\n", (void *)&a);
    printf("p: %p\n", (void *)p);
    printf("&p: %p\n", (void *)&p);
    printf("*p: %d\n", *p);
    return 0;
}
```

## 6. 코드 해석
`a`와 `*p`는 10이다. `&a`와 `p`는 같은 a object를 가리켜 같은 `%p` 표현을 보인다. `&p`는 별도 pointer object p의 address라 일반적으로 다르게 표시된다.

```text
p object                         a object
+----------------+ points to    +------+
| pointer to a   | -----------> |  10  |
+----------------+              +------+
^                               ^
| &p                            | &a and p
```

## 7. 내부 동작
[C17 abstract machine] 각 expression은 type과 designated object가 다르다. `&p`의 type은 pointer to `int *`, 즉 `int **`지만 이번 Step은 이를 저장하거나 이중 역참조하지 않고 type 관계만 확인한다. `%p`에는 object pointer를 `void *`로 변환해 전달한다.

## 8. 자주 하는 실수
- `p`와 `&p`가 같은 object를 가리킨다고 생각한다.
- `a`와 `&a`를 같은 format으로 출력한다.
- `&p`를 a의 address라고 생각한다.
- `*p`가 pointer value라고 생각한다.

## 9. 필수 실습
다섯 expressions를 type·의미·출력 format 표로 정리하고 실제 출력한다. [실습 README](../../exercises/14-pointers/14-6/README.md)

## 10. 추가 실습
- ★ 각 expression type 쓰기
- ★★ `*p` 수정 후 a 확인
- ★★★ diagram을 직접 다시 그리기

## 11. 확인 문제
1. `a`의 type은?
2. `&a`와 p의 관계는?
3. `&p`가 가리키는 object는?
4. `*p`가 지정하는 object는?
5. `&p`의 type은?
6. 어떤 expressions를 `%p`로 출력하는가?

## 12. 핵심 정리
value와 address, pointer object와 pointed-to object를 expression별 type으로 구분하면 포인터 혼동을 줄일 수 있다.

## 13. 다음 Step
[Step 14-7. 포인터 자체의 주소](14-7-pointer-object-address.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.3.2
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
