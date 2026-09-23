# 22-6. 함수 포인터 배열
## 1. 학습 목표
- array of function pointers 선언을 정확히 읽는다.
- index bounds를 검사한 뒤 dispatch한다.
- pointer-to-array 선언과 구분한다.
## 2. 선수 지식
Part 8 배열, Part 15 pointer-to-array, 22-1 function pointer 선언을 안다.
## 3. 핵심 개념
```c
int (*operations[3])(int, int);
```
identifier에서 읽으면 `operations[3]`이 먼저이므로 3 elements의 array이고, 각 element는 int 둘을 받아 int를 반환하는 function을 가리키는 pointer다.

다음 선언은 전혀 다르다.
```c
int (*p)[3];
```
`p`는 int 3개짜리 array를 가리키는 pointer다.
## 4. 문법
```c
int (*operations[])(int, int) = {add, subtract, multiply};
size_t count = sizeof operations / sizeof operations[0];
```
dispatch 전에 `index < count`를 검사한다. out-of-bounds access로 얻은 값을 function pointer처럼 호출하는 실습은 하지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

int main(void)
{
    int (*operations[])(int, int) = {add, subtract, multiply};
    size_t count = sizeof operations / sizeof operations[0];
    size_t index = 2u;

    if (index < count) {
        printf("%d\n", operations[index](6, 7));
    }
    return 0;
}
```
## 6. 코드 해석
`operations`의 각 element는 compatible function pointer다. `index`를 element count와 비교한 뒤 세 번째 pointer를 선택해 `multiply`를 호출한다.
## 7. 내부 동작
**[C17 type system]** array elements는 object type이어야 한다. function 자체는 array element가 될 수 없지만 function pointer는 object type이므로 element가 될 수 있다.

**[compiler]** array indexing과 pointer load 뒤 indirect call을 생성할 수 있다.

**[ABI]** function pointer element의 representation과 alignment는 implementation에 달려 있다.

**[CPU / ISA]** table에서 target을 선택하는 구현이 가능하지만 source 선언만으로 machine instruction 수를 고정할 수 없다.
## 8. 자주 하는 실수
- `int (*operations[3])(int, int)`를 pointer to array로 읽는다.
- function 자체의 array라고 설명한다.
- index bounds를 검사하지 않는다.
- 모든 function pointer가 object pointer와 같은 크기라고 가정해 byte 단위로 직접 다룬다.
- `operations + 1`과 `operations[1]`의 array traversal을 function pointer 자체의 `fp + 1`로 혼동한다. 전자는 array elements인 function pointer objects 사이를 이동하는 object pointer 연산이다.
## 9. 필수 실습
세 arithmetic functions의 pointer array를 만들고 유효한 index를 검사해 호출한다.
[22-6 exercise](../../exercises/22-function-pointers/22-6/README.md)
## 10. 추가 실습
- ★ divide 대신 remainder function을 추가한다.
- ★★ 유효하지 않은 index에 오류 메시지를 출력한다.
- ★★★ pointer-to-array와 array-of-function-pointers를 말로 비교한다.
## 11. 확인 문제
1. `operations[3]`가 먼저 결합하는 이유는?
2. array element가 function 자체가 아니라 function pointer인 이유는?
3. `int (*p)[3]`는 무엇인가?
4. dispatch 전에 어떤 bounds condition을 검사해야 하는가?
5. `operations + 1`이 function pointer arithmetic 자체가 아닌 이유는?
## 12. 핵심 정리
- 괄호와 postfix declarator를 따라 array of function pointers를 읽는다.
- pointer-to-array와 구분한다.
- dispatch table index는 항상 bounds를 검사한다.
## 13. 다음 Step
[22-7. state machine과 함수 포인터](22-7-state-machine-and-function-pointer.md)
## 14. 참고 자료
- N1570 6.2.5, 6.5.2.1, 6.7.6.2, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: array declaration](https://en.cppreference.com/w/c/language/array)
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
