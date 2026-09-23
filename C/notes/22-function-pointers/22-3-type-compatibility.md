# 22-3. 함수 포인터 타입 호환성
## 1. 학습 목표
- function pointer assignment와 call에 필요한 compatible function type을 설명한다.
- incompatible function pointer를 통한 호출의 undefined behavior를 구분한다.
- null function pointer를 검사한 뒤 호출한다.
## 2. 선수 지식
22-1 선언 해석과 22-2 assignment·call을 이해한다.
## 3. 핵심 개념
```c
int (*operation)(int, int);
```
`operation`에는 int 둘을 받아 int를 반환하는 compatible function type의 target을 사용해야 한다. parameter 개수나 return type이 달라도 “주소만 있으면” 호출 가능한 것이 아니다.

C17은 function을 호환되지 않는 function type의 pointer를 통해 호출하면 undefined behavior라고 규정한다. cast로 compiler 진단을 없애도 실제 call contract가 안전해지지 않는다.
## 4. 문법
```c
int add(int, int);
double average(double, double);

int (*fp)(int, int) = add; /* compatible */
```
`average`를 억지로 cast하여 `fp`로 호출하는 코드는 작성하거나 실행하지 않는다.

null pointer constant로 null function pointer를 만들 수 있다.
```c
#include <stddef.h>
int (*optional)(int, int) = NULL;
```
호출 전 `optional != NULL`을 검사한다. null function pointer는 특정 machine address 0이나 반드시 all-bits-zero representation이라는 뜻이 아니다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int subtract(int a, int b)
{
    return a - b;
}

int main(void)
{
    int (*operation)(int, int) = NULL;

    operation = subtract;
    if (operation != NULL) {
        printf("%d\n", operation(9, 4));
    }
    return 0;
}
```
## 6. 코드 해석
`operation`은 먼저 null function pointer로 초기화된다. compatible function `subtract`의 pointer value를 받은 뒤 null 여부를 검사하고 정확한 arguments로 호출한다.
## 7. 내부 동작
**[C17 type system]** compatible type 규칙은 return type과 parameter type 등을 포함한다. compatible function type을 가리키는 pointer끼리 assignment와 call contract를 맞춘다. 호환되지 않는 function type의 pointer를 통한 호출은 undefined behavior다.

**[compiler]** warning은 mismatch를 발견하는 도움일 뿐이다. 명시적 cast로 warning을 숨겨도 ABI 수준의 argument 전달과 return 처리 불일치가 해결되지 않는다.

**[ABI]** 서로 다른 signature는 register 사용, value representation, return convention이 다를 수 있다.

**[CPU / ISA]** CPU가 target으로 branch할 수 있다는 사실만으로 C function call이 유효해지는 것은 아니다.
## 8. 자주 하는 실수
- parameter 개수나 return type이 달라도 cast하면 안전하다고 생각한다.
- null function pointer를 호출한다.
- 초기화하지 않은 function pointer를 호출한다.
- `int f()`를 no-parameter prototype으로 사용한다. `int f(void)`를 사용한다.
- function pointer에 object pointer arithmetic 규칙을 적용해 `fp + 1`, `fp++`, `fp1 - fp2`를 시도한다.
- function pointers에 `<`로 일반적인 주소 순서를 만들려 한다. equality 비교 `fp == subtract`, `fp != NULL`과 구분한다.
## 9. 필수 실습
동일한 signature의 두 function 중 하나를 function pointer에 대입하고 null check 뒤 호출한다.
[22-3 exercise](../../exercises/22-function-pointers/22-3/README.md)
## 10. 추가 실습
- ★ compatible function을 하나 더 작성한다.
- ★★ `fp == subtract`로 현재 target을 확인한다.
- ★★★ incompatible 선언 예제를 실행하지 않고 type만 분석한다.
## 11. 확인 문제
1. compatible function type은 왜 call 전에 중요할까?
2. cast가 incompatible call을 안전하게 만들지 못하는 이유는?
3. null function pointer 호출은 허용되는가?
4. null pointer가 반드시 machine address 0인가?
5. function pointer에 `+ 1`을 적용할 수 없는 이유는?
6. equality comparison과 relational comparison을 어떻게 구분해야 하는가?
## 12. 핵심 정리
- function pointer와 target function type은 compatible해야 한다.
- cast는 잘못된 call contract의 해결책이 아니다.
- 모든 pointer는 초기화하고 null을 검사하며 function pointer arithmetic은 하지 않는다.
## 13. 다음 Step
[22-4. `typedef` 함수 포인터](22-4-typedef-function-pointer.md)
## 14. 참고 자료
- N1570 6.2.7, 6.3.2.3, 6.5.2.2, 6.5.6, 6.5.9, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: compatible types](https://en.cppreference.com/w/c/language/type)
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
