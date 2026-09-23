# 22-5. callback
## 1. 학습 목표
- callback을 function pointer argument로 전달하는 동작으로 설명한다.
- callback parameter도 pointer value의 pass-by-value임을 이해한다.
- callback과 captured environment를 자동으로 묶는 closure를 구분한다.
## 2. 선수 지식
Part 16 C의 pass-by-value와 22-1~22-4 function pointer를 안다.
## 3. 핵심 개념
callback은 호출할 동작을 고정하지 않고 function pointer argument로 전달해, 받은 쪽이 약속된 시점에 호출하는 방식이다.
```c
int calculate(
    int a,
    int b,
    int (*operation)(int, int)
);
```
`calculate(3, 4, add)`에서 흐름은 다음과 같다.
```text
add function designator
→ function pointer value로 변환
→ operation parameter object로 값 복사
→ operation(a, b)
```
C는 callback에서 call-by-reference로 바뀌지 않는다.
## 4. 문법
```c
int calculate(int a, int b, int (*operation)(int, int))
{
    return operation(a, b);
}
```
C function pointer 자체에는 C++ lambda처럼 captured environment가 자동 저장되지 않는다. 별도 context pointer API는 가능한 설계지만 이번 Step의 핵심 범위에는 포함하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int subtract(int a, int b)
{
    return a - b;
}

int calculate(int a, int b, int (*operation)(int, int))
{
    return operation(a, b);
}

int main(void)
{
    printf("%d\n", calculate(8, 3, add));
    printf("%d\n", calculate(8, 3, subtract));
    return 0;
}
```
## 6. 코드 해석
`calculate`의 algorithm은 같고 전달된 pointer value만 달라진다. 첫 call은 `add`, 두 번째 call은 `subtract`를 callback으로 호출한다.
## 7. 내부 동작
**[C17 type system]** function pointer argument도 다른 scalar argument처럼 parameter object에 값으로 전달된다.

**[compiler]** callback target이 compile time에 확정되지 않으면 indirect call을 생성할 수 있다.

**[ABI]** callback과 caller는 compatible function type의 calling convention을 따라야 한다.

**[CPU / ISA]** 구현은 전달된 target을 이용해 control flow를 옮길 수 있지만 C17은 특정 instruction을 지정하지 않는다.
## 8. 자주 하는 실수
- callback을 call-by-reference라고 설명한다.
- incompatible function을 cast해서 callback으로 전달한다.
- function pointer에 captured local variables가 자동 저장된다고 생각한다.
- ISO C17에 nested function이 있다고 생각한다. GCC nested function은 extension이며 사용하지 않는다.
- C++ lambda, `std::function`, reference parameter를 C17 기능으로 섞는다.
## 9. 필수 실습
add·subtract·multiply 중 compatible function을 calculator callback으로 전달한다.
[22-5 exercise](../../exercises/22-function-pointers/22-5/README.md)
## 10. 추가 실습
- ★ multiply callback을 추가한다.
- ★★ 입력한 operator에 따라 callback을 선택한다.
- ★★★ callback parameter가 pass-by-value인 흐름을 그림으로 정리한다.
## 11. 확인 문제
1. callback은 무엇을 argument로 전달하는 방식인가?
2. `add`는 `calculate` 호출에서 어떤 conversion을 거치는가?
3. callback parameter 전달이 pass-by-value인 이유는?
4. function pointer 자체가 closure가 아닌 이유는?
5. ISO C17에서 nested function을 사용할 수 있는가?
## 12. 핵심 정리
- callback은 compatible function pointer value로 동작을 전달한다.
- C의 callback parameter도 pass-by-value다.
- function pointer만으로 captured environment가 생기지 않는다.
## 13. 다음 Step
[22-6. 함수 포인터 배열](22-6-function-pointer-array.md)
## 14. 참고 자료
- N1570 6.3.2.1, 6.5.2.2, 6.9.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: function call operator](https://en.cppreference.com/w/c/language/operator_other)
- [GCC: nested functions extension](https://gcc.gnu.org/onlinedocs/gcc/Nested-Functions.html)
