# 14-5. 포인터를 통한 값 수정

`*pointer`가 modifiable object를 지정하면 assignment를 통해 그 pointed-to object의 value를 바꿀 수 있다.

## 1. 학습 목표
- `*pointer = value;`의 assignment 대상을 설명한다.
- pointer object와 pointed-to object의 변경을 구분한다.
- 여러 aliases가 같은 object를 관찰할 수 있음을 이해한다.

## 2. 선수 지식
Part 7의 assignment와 Step 14-4의 indirection을 사용한다.

## 3. 핵심 개념
`pointer = &number`일 때 `*pointer`는 `number` object를 지정한다. 따라서 `*pointer = 20;`은 pointer 안의 pointer value를 바꾸는 것이 아니라 `number`의 int value를 20으로 바꾼다. pointer는 object를 소유하지 않고 가리키는 value를 저장한다.

## 4. 문법
```c
int number = 10;
int *pointer = &number;
*pointer = 20;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int number = 10;
    int *pointer = &number;

    printf("before: %d\n", number);
    *pointer = 20;
    printf("after: %d\n", number);
    return 0;
}
```

## 6. 코드 해석
처음 `number`는 10이다. `*pointer`가 `number`를 지정하므로 assignment 후 `number`를 직접 읽어도 20이다. pointer value 자체는 여전히 같은 object를 가리킨다.

## 7. 내부 동작
[C17 abstract machine] indirection expression이 modifiable lvalue인 object를 지정하면 assignment가 그 object의 stored value를 바꾼다. [compiler/CPU] 실제 store instruction 사용 여부와 형태는 최적화와 target에 달리며 C statement 하나와 instruction 하나가 반드시 대응하지 않는다.

## 8. 자주 하는 실수
- `*pointer = 20`이 pointer value를 20으로 바꾼다고 생각한다.
- pointed-to object와 pointer object를 같은 object라고 생각한다.
- invalid pointer를 assignment 왼쪽에서 dereference한다.
- pointer가 number storage를 소유한다고 설명한다.

## 9. 필수 실습
pointer로 int object 값을 두 번 수정하고 object를 직접 읽어 결과를 확인한다. [실습 README](../../exercises/14-pointers/14-5/README.md)

## 10. 추가 실습
- ★ double object 수정
- ★★ 두 aliases 중 하나로 수정하고 다른 하나로 읽기
- ★★★ object/pointer value 변화표 작성

## 11. 확인 문제
1. `*pointer = 20`의 assignment 대상은?
2. pointer value도 바뀌는가?
3. `number`를 직접 읽으면 어떤 값인가?
4. 두 pointers가 같은 object를 가리킬 수 있는가?
5. pointer가 object를 소유하는가?

## 12. 핵심 정리
valid pointer의 indirection은 pointed-to object를 지정하므로 assignment가 그 object의 value를 직접 바꾼다.

## 13. 다음 Step
[Step 14-6. `a`, `&a`, `p`, `&p`, `*p`](14-6-five-expressions.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.2, 6.5.16
- [cppreference: Assignment operators](https://en.cppreference.com/w/c/language/operator_assignment.html)
