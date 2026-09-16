# 15-5. pointer arithmetic의 원소 단위

object pointer에 integer를 더하거나 빼는 연산은 같은 array object 안에서 pointed-to type의 element positions를 기준으로 한다.

## 1. 학습 목표
- `int *`와 `double *`의 다음 element 의미를 설명한다.
- pointer arithmetic을 raw byte arithmetic과 구분한다.
- operation의 array bounds 전제를 유지한다.

## 2. 선수 지식
Step 15-4의 element pointers와 Part 14의 pointer types를 사용한다.

## 3. 핵심 개념
`int *p`에서 `p+1`은 같은 int array의 다음 int element, `double *q`에서 `q+1`은 같은 double array의 다음 double element를 가리킨다. 이를 각각 4 bytes, 8 bytes 이동이라고 C17 규칙으로 암기하면 안 된다. type sizes는 implementation-dependent이고 언어 의미는 element positions다.

## 4. 문법
```c
pointer + integer
integer + pointer
pointer - integer
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int integers[3] = {10, 20, 30};
    double reals[3] = {1.5, 2.5, 3.5};
    int *int_pointer = integers;
    double *double_pointer = reals;

    printf("%d %d\n", *int_pointer, *(int_pointer + 1));
    printf("%.1f %.1f\n", *double_pointer, *(double_pointer + 1));
    printf("%zu %zu\n", sizeof(*int_pointer), sizeof(*double_pointer));
    return 0;
}
```

## 6. 코드 해석
각 pointer의 +1은 자기 array의 second element를 가리켜 20과 2.5를 읽는다. 마지막 줄은 pointed-to types의 sizes를 관찰하지만 이 숫자가 pointer arithmetic의 정의 그 자체는 아니다.

## 7. 내부 동작
[C17 abstract machine] pointer addition result는 같은 array object의 element 또는 one-past 범위에 있을 때 정의된다. [compiler/ABI] 실제 address calculation이 scaled arithmetic으로 구현될 수 있다. [CPU] instruction 형태는 target과 optimization에 달리며 C가 byte multiplication 공식을 직접 요구하지 않는다.

## 8. 자주 하는 실수
- p+1을 address 숫자 +1로 설명한다.
- int와 double sizes를 모든 구현에서 4와 8로 고정한다.
- unrelated memory로 pointer를 자유롭게 이동한다.
- bounds 밖 pointer를 만든 뒤 dereference하지 않으면 항상 괜찮다고 생각한다.

## 9. 필수 실습
int와 double arrays에서 pointer +1로 next element를 읽고 type별 의미를 설명한다. [실습 README](../../exercises/15-arrays-and-pointers/15-5/README.md)

## 10. 추가 실습
- ★ char array next element
- ★★ pointer +2로 third element
- ★★★ C semantic과 가상 sizeof 기반 address 계산 비교

## 11. 확인 문제
1. `int *p`의 p+1은 무엇을 가리키는가?
2. `double *q`의 q+1은?
3. C가 int size 4를 보장하는가?
4. pointer arithmetic의 정의 영역은?
5. actual CPU instruction이 항상 같은가?

## 12. 핵심 정리
pointer arithmetic은 pointed-to type의 array element 단위이며 같은 array와 one-past bounds 안에서 해석한다.

## 13. 다음 Step
[Step 15-6. 같은 배열과 one-past 경계](15-6-one-past-boundary.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
