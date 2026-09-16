# 15-4. `&arr[i]`와 `arr + i`

유효한 index i에 대해 `&arr[i]`와 `arr + i`는 같은 i번째 element를 가리키는 pointer values다.

## 1. 학습 목표
- element address와 pointer addition 결과를 연결한다.
- pointer value 계산과 dereference를 구분한다.
- bounds와 one-past 경계를 예고한다.

## 2. 선수 지식
Step 15-3의 subscript definition과 Part 14의 address operator를 안다.

## 3. 핵심 개념
`arr[i]`가 i번째 element를 지정하므로 `&arr[i]`는 그 element의 address다. `arr + i`도 converted first element pointer에서 i elements 이동해 같은 pointer를 만든다. i가 count이면 one-past pointer는 계산할 수 있지만 해당 object를 dereference할 수 없다.

## 4. 문법
```c
&arr[i]
arr + i
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[4] = {10, 20, 30, 40};

    for (int i = 0; i < 4; ++i) {
        printf("%p %p %d\n",
               (void *)&values[i],
               (void *)(values + i),
               *(values + i));
    }
    return 0;
}
```

## 6. 코드 해석
각 i에서 두 pointer expressions가 같은 element를 가리켜 같은 `%p` 표현을 보인다. 세 번째 output은 그 valid pointer를 dereference한 element 값이다.

## 7. 내부 동작
[C17 abstract machine] `&arr[i]`와 `arr+i`의 pointer relationship은 subscript와 additive operator 규칙에서 나온다. [implementation] 주소 숫자의 간격은 `sizeof(int)`에 대응하는 구현 결과로 보일 수 있으나 pointer arithmetic 의미는 다음 int element이지 숫자 +1이 아니다.

## 8. 자주 하는 실수
- p+1을 address integer에 1 더하기라고 설명한다.
- 출력 주소 차이를 C17 고정 byte 수로 일반화한다.
- one-past pointer를 dereference한다.
- unrelated objects 사이의 pointer arithmetic도 같다고 생각한다.

## 9. 필수 실습
각 element의 `&arr[i]`와 `arr+i`를 비교하고 valid value를 읽는다. [실습 README](../../exercises/15-arrays-and-pointers/15-4/README.md)

## 10. 추가 실습
- ★ char array addresses
- ★★ double array addresses
- ★★★ 가상 sizeof 가정과 실제 관찰 구분

## 11. 확인 문제
1. `&arr[i]`는 무엇을 가리키는가?
2. `arr+i`와 어떤 관계인가?
3. p+1의 C 의미는?
4. i가 count일 때 pointer 계산은 가능한가?
5. 그 pointer를 dereference할 수 있는가?

## 12. 핵심 정리
element address와 converted pointer arithmetic은 같은 element를 가리키며 이동 단위는 pointed-to array element다.

## 13. 다음 Step
[Step 15-5. pointer arithmetic의 원소 단위](15-5-pointer-arithmetic-element-unit.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.3.2, 6.5.6
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
