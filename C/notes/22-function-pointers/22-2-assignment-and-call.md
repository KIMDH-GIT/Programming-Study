# 22-2. 함수 포인터 대입과 호출
## 1. 학습 목표
- function designator의 function-to-pointer conversion을 설명한다.
- `f`와 `&f`, `fp()`와 `(*fp)()`의 관계를 구분한다.
- direct call과 function pointer를 통한 call을 C17과 구현 층위에서 나눈다.
## 2. 선수 지식
22-1 function과 pointer-to-function type을 이해한다.
## 3. 핵심 개념
function 이름을 expression에서 사용하면 대부분의 context에서 function designator가 pointer to function으로 변환된다.
```c
int add(int, int);
int (*fp)(int, int) = add;
```
여기서 `add`의 type 자체가 항상 function pointer인 것은 아니다. `add`는 function designator이고 initializer context에서 pointer value로 변환된다.

unary `&`의 operand가 function designator이면 conversion 없이 function의 주소를 만든다. 따라서 다음 두 대입은 모두 가능하다.
```c
fp = add;
fp = &add;
```
그러나 변환 전 `add`는 function type이고 `&add` expression은 pointer-to-function type이므로 “둘의 type이 완전히 같다”고 설명하면 안 된다.
## 4. 문법
```c
int result1 = fp(3, 4);
int result2 = (*fp)(3, 4);
```
function call operator의 왼쪽 operand는 pointer to function이어야 한다. `(*fp)`는 function designator가 되고 call context에서 다시 pointer로 변환되므로 두 형태는 같은 function을 호출한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int main(void)
{
    int (*operation)(int, int) = add;
    int first = operation(3, 4);

    operation = &add;
    printf("%d %d\n", first, (*operation)(5, 6));
    return 0;
}
```
## 6. 코드 해석
첫 initializer에서는 `add`가 function pointer value로 변환된다. 두 번째 대입의 `&add`는 unary `&`로 pointer value를 얻는다. `operation(...)`과 `(*operation)(...)`은 각각 같은 target을 올바른 arguments로 호출한다.
## 7. 내부 동작
**[C17 type system]** function designator는 `sizeof`와 unary `&`의 operand일 때를 제외하고 pointer to function으로 변환된다. call operator는 pointer to function을 통해 호출한다.

**[compiler]** source가 특정 function을 직접 지정한 `add(1, 2)`는 direct call로, runtime pointer value를 통한 `fp(1, 2)`는 indirect call로 구현될 수 있다. target이 알려지면 optimizer가 indirect call을 direct call처럼 최적화할 수도 있으므로 “함수 포인터는 항상 느리다”고 단정할 수 없다.

**[ABI]** function pointer 표현과 call sequence는 target ABI가 정한다.

**[CPU / ISA]** `(*fp)`가 반드시 별도의 memory dereference instruction을 만든다는 뜻은 아니다.

**[MIPS — 수업 기준]** 구현의 indirect call은 `jalr` 같은 indirect control transfer와 연결될 수 있으나 항상 instruction 하나로 고정되지는 않는다.

**[RISC-V — 병행 학습]** indirect call은 `jalr`와 연결될 수 있지만 compiler와 ABI에 따라 실제 sequence가 달라질 수 있다.
## 8. 자주 하는 실수
- function 이름의 type이 언제나 function pointer라고 말한다.
- `add`와 `&add`가 변환 전부터 같은 type이라고 말한다.
- `fp()`와 `(*fp)()`가 서로 다른 target을 호출한다고 생각한다.
- `*fp`를 function object의 memory contents를 load한다고 설명한다. C에서 function은 object가 아니다.
- function pointer를 `%p`에 `(void *)` cast하여 portable하게 출력할 수 있다고 단정한다. `%p`는 `void *` argument용이며 ISO C17은 object pointer와 function pointer conversion을 동일하게 보장하지 않는다.
## 9. 필수 실습
같은 function pointer에 `f`와 `&f`를 차례로 대입하고 `fp()`와 `(*fp)()` 결과를 비교한다.
[22-2 exercise](../../exercises/22-function-pointers/22-2/README.md)
## 10. 추가 실습
- ★ 다른 compatible function을 대입한다.
- ★★ direct call과 pointer call 결과를 비교한다.
- ★★★ C17 conversion이 억제되는 두 context를 설명한다.
## 11. 확인 문제
1. initializer의 `add`에는 어떤 conversion이 일어나는가?
2. `&add`에서 function-to-pointer conversion이 먼저 일어나는가?
3. `add`와 `&add`의 변환 전 type은 왜 같다고 할 수 없는가?
4. `fp()`와 `(*fp)()`가 모두 가능한 이유는?
5. 함수 포인터 호출이 항상 느리다고 단정할 수 없는 이유는?
6. `(void *)fp`와 `%p`를 portable C 예제로 삼지 않는 이유는?
## 12. 핵심 정리
- function designator와 pointer-to-function expression을 구분한다.
- `f`와 `&f`는 모두 유효한 pointer value를 제공할 수 있다.
- `fp()`와 `(*fp)()`는 같은 call semantics를 갖는다.
## 13. 다음 Step
[22-3. 함수 포인터 타입 호환성](22-3-type-compatibility.md)
## 14. 참고 자료
- N1570 6.3.2.1, 6.5.2.2, 6.5.3.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: value transformations](https://en.cppreference.com/w/c/language/conversion)
- [cppreference: function call operator](https://en.cppreference.com/w/c/language/operator_other)
