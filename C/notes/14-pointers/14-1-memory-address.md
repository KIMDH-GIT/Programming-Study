# 14-1. 메모리 주소란 무엇인가

C에서 object는 lifetime 동안 storage를 차지하며, address는 그 object를 가리키는 pointer value와 연결되는 언어 개념이다.

## 1. 학습 목표
- object·identifier·value·address를 구분한다.
- C pointer value와 물리 RAM 번호를 동일시하지 않는다.
- `%p`로 object address를 올바르게 관찰한다.

## 2. 선수 지식
Part 2의 object와 type, Part 5의 `%p`, Part 10의 lifetime을 사용한다.

## 3. 핵심 개념
`int number = 10;`에서 `number`는 identifier, 선언으로 만들어진 저장 대상은 `int` object, 10은 저장된 value다. `&number`는 그 object를 가리키는 pointer value를 만든다. 입문 수준에서 이를 address라고 부르지만 C17은 pointer가 단순 integer 물리 주소와 같은 표현이라고 요구하지 않는다.

## 4. 문법
```c
int number = 10;
printf("%p\n", (void *)&number);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;

    printf("value: %d\n", number);
    printf("address: %p\n", (void *)&number);
    return 0;
}
```

## 6. 코드 해석
첫 줄은 `number` object에 저장된 int value 10을 출력한다. 둘째 줄은 `number`를 가리키는 pointer value를 `%p` 형식으로 표시한다. 표시 문자열은 실행 환경과 실행 시점에 따라 달라질 수 있다.

## 7. 내부 동작
[C17 abstract machine] unary `&`는 operand object를 가리키는 pointer를 만든다. [compiler/ABI] pointer representation과 크기는 구현이 정한다. [OS] hosted 환경에서는 virtual address와 관련될 수 있지만 C17은 virtual memory를 요구하지 않는다. [CPU] 실제 load/store 구현은 compiler와 target에 달린다.

## 8. 자주 하는 실수
- identifier와 object를 같은 개념으로만 설명한다.
- pointer value를 단순 integer 주소 숫자라고 단정한다.
- `&number`를 물리 RAM 주소라고 설명한다.
- pointer를 `%d`나 `%ld`로 출력한다.

## 9. 필수 실습
두 int objects의 values와 addresses를 `%d`, `%p`로 구분해 출력한다. [실습 README](../../exercises/14-pointers/14-1/README.md)

## 10. 추가 실습
- ★ 한 char object의 address 출력
- ★★ 같은 object address를 두 번 관찰
- ★★★ C17·ABI·OS·CPU 설명을 표로 구분

## 11. 확인 문제
1. identifier와 object의 차이는?
2. `&number`가 만드는 값의 종류는?
3. `%p` argument에는 어떤 conversion을 사용하는가?
4. C17이 virtual memory를 요구하는가?
5. pointer representation이 반드시 integer와 같은가?

## 12. 핵심 정리
address는 object를 가리키는 pointer value로 다루며 실제 표현과 virtual·physical memory는 구현 관점으로 분리한다.

## 13. 다음 Step
[Step 14-2. 주소 연산자 `&`](14-2-address-operator.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.3.2
- [cppreference: Address-of operator](https://en.cppreference.com/w/c/language/operator_member_access.html#Address_of)
