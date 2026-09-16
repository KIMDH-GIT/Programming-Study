# 14-10. 포인터 타입과 접근형

pointer type은 어떤 type의 object를 가리키고 indirection으로 어떤 type의 object를 지정하는지 표현한다.

## 1. 학습 목표
- `int *`와 `double *`의 pointed-to types를 구분한다.
- `sizeof(pointer)`와 `sizeof(*pointer)`를 구분한다.
- pointer representation size를 C17 상수처럼 가정하지 않는다.

## 2. 선수 지식
Step 14-3의 declarations와 Part 2의 `sizeof`를 사용한다.

## 3. 핵심 개념
`int *p`를 dereference하면 `int` object를, `double *q`를 dereference하면 `double` object를 지정한다. pointer type은 단순히 “주소 크기”가 아니라 access type 관계를 제공한다. `sizeof(p)`는 pointer object type의 크기, `sizeof(*p)`는 pointed-to `int` type의 크기다.

## 4. 문법
```c
int *int_pointer = &integer;
double *double_pointer = &real;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int integer = 10;
    double real = 2.5;
    int *int_pointer = &integer;
    double *double_pointer = &real;

    printf("%d %.1f\n", *int_pointer, *double_pointer);
    printf("int pointer: %zu\n", sizeof(int_pointer));
    printf("int object: %zu\n", sizeof(*int_pointer));
    printf("double pointer: %zu\n", sizeof(double_pointer));
    printf("double object: %zu\n", sizeof(*double_pointer));
    return 0;
}
```

## 6. 코드 해석
각 pointer는 맞는 object type을 읽어 10과 2.5를 출력한다. pointer sizes와 pointed object sizes를 별도로 관찰한다. 특정 결과가 같거나 8이라고 해도 C17의 모든 구현에 일반화하지 않는다.

## 7. 내부 동작
[C17 abstract machine] pointer to different referenced types are distinct pointer types. [compiler/ABI] 여러 object pointer types가 같은 representation/size인 ABI가 흔하지만 이 관찰만으로 모든 pointer types가 항상 같다고 단정하지 않는다. incompatible pointer assignment에는 diagnostic이 필요할 수 있다.

## 8. 자주 하는 실수
- 모든 pointer types를 같은 type이라고 생각한다.
- `sizeof(pointer)`를 pointed object 크기라고 생각한다.
- pointer는 항상 8 bytes라고 말한다.
- incompatible pointer를 cast로 숨기면 access가 자동으로 안전해진다고 생각한다.

## 9. 필수 실습
int와 double pointers의 dereference values와 pointer/object sizes를 구분해 출력한다. [실습 README](../../exercises/14-pointers/14-10/README.md)

## 10. 추가 실습
- ★ char pointer와 char object 크기
- ★★ 세 pointer sizes 관찰 후 구현 결과로 표기
- ★★★ type mismatch 코드를 실행 없이 diagnostic 분석

## 11. 확인 문제
1. `int *` dereference 결과 object type은?
2. `sizeof(p)`는 무엇의 크기인가?
3. `sizeof(*p)`는 무엇의 크기인가?
4. 모든 pointer가 8 bytes인가?
5. size가 같으면 pointer types도 같은가?

## 12. 핵심 정리
pointer type은 access 대상 type을 나타내며 pointer object 크기와 pointed-to object 크기, 표준 보장과 ABI 관찰을 구분한다.

## 13. 다음 Step
[Step 14-11. 잘못된 포인터 접근](14-11-invalid-pointer-access.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.3.2.3, 6.5.3.2, 6.5.3.4
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
