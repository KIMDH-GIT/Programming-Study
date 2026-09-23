# 22-4. `typedef` 함수 포인터
## 1. 학습 목표
- raw function pointer declarator를 먼저 해석한 뒤 typedef name으로 줄인다.
- pointer-to-function type의 typedef와 function definition을 구분한다.
- typedef를 사용해 callback parameter를 읽기 쉽게 선언한다.
## 2. 선수 지식
Part 20 `typedef`와 22-1~22-3 function pointer type을 이해한다.
## 3. 핵심 개념
먼저 raw 선언을 읽는다.
```c
int (*operation)(int, int);
```
그 구조에 typedef name을 붙이면 다음과 같다.
```c
typedef int (*BinaryOperation)(int, int);
BinaryOperation operation;
```
`BinaryOperation`은 pointer-to-function type의 typedef name이다. 새로운 function object를 만들거나 function을 정의하는 문법이 아니다.
## 4. 문법
```c
typedef int (*BinaryOperation)(int, int);

int calculate(int a, int b, BinaryOperation operation);
```
typedef는 선언을 짧게 하지만 underlying type compatibility를 바꾸지 않는다. `BinaryOperation` target은 여전히 int 둘을 받고 int를 반환해야 한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef int (*BinaryOperation)(int, int);

int multiply(int a, int b)
{
    return a * b;
}

int calculate(int a, int b, BinaryOperation operation)
{
    return operation(a, b);
}

int main(void)
{
    BinaryOperation selected = multiply;

    printf("%d\n", calculate(6, 7, selected));
    return 0;
}
```
## 6. 코드 해석
`BinaryOperation`은 compatible function을 가리키는 pointer type의 별칭이다. `selected`는 pointer object이며 `calculate`에는 그 pointer value가 복사되어 전달된다.
## 7. 내부 동작
**[C17 type system]** typedef name은 기존 type의 synonym이며 별개의 호환성 규칙을 만들지 않는다.

**[compiler]** typedef를 펼쳐 raw declarator와 같은 type으로 검사한다.

**[ABI]** typedef 사용 여부는 call convention을 바꾸지 않는다.

**[CPU / ISA]** source-level 별칭은 별도 runtime operation을 요구하지 않는다.
## 8. 자주 하는 실수
- raw `(*)(...)` 구조를 이해하기 전에 typedef만 외운다.
- typedef가 새 function을 생성한다고 생각한다.
- `typedef int BinaryOperation(int, int);`와 pointer typedef를 구분하지 않는다. 전자는 function type의 typedef이고 여기서 사용하는 것은 `typedef int (*BinaryOperation)(int, int);`이다.
- typedef를 쓰면 incompatible function도 대입할 수 있다고 생각한다.
## 9. 필수 실습
raw function pointer 선언을 먼저 작성한 뒤 동일한 type의 typedef name으로 calculator를 구현한다.
[22-4 exercise](../../exercises/22-function-pointers/22-4/README.md)
## 10. 추가 실습
- ★ subtract function을 같은 typedef로 호출한다.
- ★★ raw 선언과 typedef 선언을 나란히 적는다.
- ★★★ function type typedef와 pointer-to-function typedef의 차이를 분석한다.
## 11. 확인 문제
1. `BinaryOperation`은 object인가 type name인가?
2. pointer-to-function typedef의 `*`는 어디에 있는가?
3. typedef가 function type compatibility를 바꾸는가?
4. `calculate`의 operation parameter에는 무엇이 전달되는가?
5. raw declarator를 먼저 학습하는 이유는?
## 12. 핵심 정리
- typedef는 pointer-to-function type에 읽기 쉬운 이름을 붙인다.
- typedef name은 function이나 object를 자동 생성하지 않는다.
- typedef를 사용해도 signature compatibility는 그대로다.
## 13. 다음 Step
[22-5. callback](22-5-callback.md)
## 14. 참고 자료
- N1570 6.7.8, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: typedef declaration](https://en.cppreference.com/w/c/language/typedef)
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
