# 22-1. 함수 주소와 함수 포인터 선언
## 1. 학습 목표
- function과 function pointer object를 구분한다.
- `int (*operation)(int, int)` 선언을 identifier에서 바깥쪽으로 읽는다.
- object pointer와 function pointer를 같은 범주로 취급하지 않는다.
## 2. 선수 지식
Part 10 함수 선언·정의, Part 14 pointer, Part 15 declarator 괄호를 안다.
## 3. 핵심 개념
```c
int add(int a, int b)
{
    return a + b;
}
```
`add`는 function이다. 반면 다음 선언의 `operation`은 pointer object다.
```c
int (*operation)(int, int);
```
identifier `operation`에서 시작하면 `(*operation)`은 operation이 pointer임을, 바깥의 `(int, int)`는 pointed-to type이 두 int를 받는 function type임을, 맨 왼쪽 `int`는 그 function의 return type임을 나타낸다.

C17 언어 수준에서 function pointer에는 function을 가리킬 수 있는 pointer value가 저장된다. 이를 항상 특정 text segment의 숫자 주소라고 단정할 수 없다.
## 4. 문법
```c
int (*fp)(int); /* pointer to function taking int and returning int */
int *f(int);    /* function taking int and returning pointer to int */
```
postfix `()`가 `*`보다 declarator에서 더 강하게 결합하므로 첫 선언에는 괄호가 필요하다. 괄호가 없으면 `fp`가 function으로 해석된다.

parameter 이름은 선언에서 생략할 수 있다.
```c
int (*operation)(int, int);
```
매개변수가 없다는 prototype은 `void (*callback)(void)`처럼 `void`로 명확히 쓴다. `int f()`는 C17에서 “매개변수가 없는 prototype”이 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int square(int value)
{
    return value * value;
}

int main(void)
{
    int (*operation)(int) = square;

    printf("%d\n", operation(5));
    return 0;
}
```
## 6. 코드 해석
`square`는 int 하나를 받아 int를 반환하는 function이다. `operation`은 그와 호환되는 function type을 가리키는 pointer object이며, initializer로 유효한 function pointer value를 받는다.
## 7. 내부 동작
**[C17 type system]** function type과 pointer-to-function type은 서로 다른 type이다. function은 object가 아니지만 function pointer 변수는 값을 저장하는 object다.

**[compiler]** compiler는 선언과 initializer의 type compatibility를 검사하고 호출 코드를 생성한다.

**[ABI]** 특정 target은 function pointer를 code address나 descriptor 등으로 표현할 수 있다.

**[CPU / ISA]** 구현은 indirect control transfer를 사용할 수 있지만 C17이 특정 instruction이나 pointer 크기를 요구하지 않는다.
## 8. 자주 하는 실수
- function 자체를 pointer 변수라고 설명한다.
- `int (*fp)(int)`와 `int *fp(int)`를 같은 선언으로 읽는다.
- function pointer가 항상 8 bytes이고 모든 pointer가 같은 크기라고 단정한다.
- function pointer에 실제 숫자 주소만 저장된다고 단정한다.
- `sizeof square`처럼 function에 `sizeof`를 적용한다. function type에는 `sizeof`를 적용할 수 없지만 `sizeof operation`은 pointer object의 크기를 구한다.
## 9. 필수 실습
호환되는 function을 선언하고 raw function pointer declarator를 직접 작성해 호출한다.
[22-1 exercise](../../exercises/22-function-pointers/22-1/README.md)
## 10. 추가 실습
- ★ `cube` function과 그 pointer를 선언한다.
- ★★ `int (*f)(int)`와 `int *f(int)`를 말로 해석한다.
- ★★★ `int (*p[3])(int)`와 `int (*p)[3]`를 선언만 보고 구분한다.
## 11. 확인 문제
1. `add`와 `int (*operation)(int, int)`에서 object인 것은 무엇인가?
2. `int (*fp)(int)`를 identifier에서 바깥쪽으로 읽으면?
3. `int *fp(int)`는 무엇을 선언하는가?
4. 함수 포인터의 크기를 8 bytes로 고정해 설명할 수 없는 이유는?
5. `sizeof square`와 `sizeof operation`의 차이는?
## 12. 핵심 정리
- function과 function pointer object는 다르다.
- `(*identifier)` 괄호가 pointer-to-function 선언을 만든다.
- C17 semantics와 compiler·ABI·CPU 구현 설명을 분리한다.
## 13. 다음 Step
[22-2. 함수 포인터 대입과 호출](22-2-assignment-and-call.md)
## 14. 참고 자료
- N1570 6.2.5, 6.5.3.4, 6.7.6.1, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
- [cppreference: function declaration](https://en.cppreference.com/w/c/language/function_declaration)
