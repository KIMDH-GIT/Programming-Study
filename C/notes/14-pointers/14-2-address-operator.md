# 14-2. 주소 연산자 `&`

unary address operator `&`는 유효한 object operand를 가리키는 pointer value를 만든다.

## 1. 학습 목표
- `&object` 결과의 pointer type을 설명한다.
- pointer value를 pointer object에 저장한다.
- address-of를 bitwise AND와 구분한다.

## 2. 선수 지식
Step 14-1의 object address와 Part 7의 operator 문맥을 안다.

## 3. 핵심 개념
`int number`에 `&number`를 적용하면 type `int *`인 pointer value가 만들어진다. `int *pointer = &number;`는 그 값을 pointer object에 저장한다. `&`는 operand 수와 문맥에 따라 unary address operator 또는 binary bitwise AND가 되지만 이번 Step은 unary form이다.

## 4. 문법
```c
type object = initial_value;
type *pointer = &object;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;
    int *pointer = &number;

    printf("&number: %p\n", (void *)&number);
    printf("pointer: %p\n", (void *)pointer);
    return 0;
}
```

## 6. 코드 해석
`&number`가 `number`를 가리키는 pointer value를 만들고 `pointer`가 그 값을 저장한다. 두 `%p` 출력은 같은 object를 가리키므로 같은 표현으로 비교 관찰된다.

```text
number object
+------+
|  10  |
+------+
   ^
   |
pointer object stores a pointer value to number
```

## 7. 내부 동작
[C17 abstract machine] `&number`의 result type은 pointer to the type of `number`다. [implementation] 출력 표현은 `%p`가 정하며 숫자 형식이나 길이는 implementation-defined display 형식이다. pointer object가 pointed-to object를 소유하거나 lifetime을 연장하지 않는다.

## 8. 자주 하는 실수
- `&number`가 number의 value를 반환한다고 생각한다.
- pointer object와 pointer value를 구분하지 않는다.
- pointer가 pointed-to storage를 소유한다고 설명한다.
- `int *pointer = number;`처럼 int value를 pointer에 넣는다.

## 9. 필수 실습
int object의 address를 pointer에 저장하고 `&object`와 pointer를 모두 `%p`로 출력한다. [실습 README](../../exercises/14-pointers/14-2/README.md)

## 10. 추가 실습
- ★ double object address 저장
- ★★ 두 pointer objects가 같은 int object를 가리키게 하기
- ★★★ object/pointer object/pointer value diagram 작성

## 11. 확인 문제
1. `&number`의 result type은?
2. `pointer` 자체는 object인가 value인가?
3. pointer 안에는 무엇이 저장되는가?
4. pointer가 number의 lifetime을 연장하는가?
5. unary `&`와 binary `&`는 같은 문법 역할인가?

## 12. 핵심 정리
`&`는 object를 가리키는 typed pointer value를 만들고 pointer object는 그 값을 저장한다.

## 13. 다음 Step
[Step 14-3. 포인터 변수 선언](14-3-pointer-declaration.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.2, 6.7.6
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
