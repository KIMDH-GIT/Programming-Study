# 15-1. 배열 이름과 첫 원소 주소

array object와 pointer object는 다르지만, array expression은 여러 context에서 첫 element를 가리키는 pointer로 변환된다.

## 1. 학습 목표
- declared array type과 converted pointer type을 구분한다.
- `values`와 `&values[0]`의 관계를 설명한다.
- array-to-pointer conversion이 expression 규칙임을 이해한다.

## 2. 선수 지식
Part 11의 array object·element와 Part 14의 pointer value·`&`를 사용한다.

## 3. 핵심 개념
`int values[5]`로 선언한 object의 type은 array of 5 int다. 많은 expression context에서 `values` expression은 첫 element `values[0]`을 가리키는 `int *` value로 변환된다. array object 자체가 pointer object로 바뀌거나 배열 안에 주소가 저장되는 것은 아니다.

## 4. 문법
```c
int values[5] = {10, 20, 30, 40, 50};
int *first = values;       /* conversion */
int *same = &values[0];
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *first = values;

    printf("values: %p\n", (void *)values);
    printf("&values[0]: %p\n", (void *)&values[0]);
    printf("first value: %d\n", *first);
    return 0;
}
```

## 6. 코드 해석
`values`가 initializer의 오른쪽에서 `int *`로 변환되어 `first`에 저장된다. `%p` 출력에서 converted `values`와 `&values[0]`이 같은 첫 element를 가리킨다. `*first`는 10이다.

## 7. 내부 동작
[C17 abstract machine] array expression은 `sizeof`, unary `&` 등 예외 context를 제외하면 first element pointer로 변환된다. [compiler/ABI] 표시되는 address representation은 구현에 달린다. 같은 표현을 관찰해도 declared array type과 converted pointer type이 같다는 뜻은 아니다.

## 8. 자주 하는 실수
- 배열 object 자체가 pointer라고 말한다.
- 배열 이름 안에 첫 element 주소가 저장된다고 생각한다.
- conversion이 array object의 type을 영구히 바꾼다고 생각한다.
- address 출력이 같으면 모든 expression 의미도 같다고 결론 낸다.

## 9. 필수 실습
array expression과 `&array[0]`을 출력하고 pointer로 첫 element를 읽는다. [실습 README](../../exercises/15-arrays-and-pointers/15-1/README.md)

## 10. 추가 실습
- ★ double array의 first element
- ★★ conversion 전 declared type과 expression result type 표
- ★★★ conversion 발생·비발생 context 분류

## 11. 확인 문제
1. `int values[5]`의 declared object type은?
2. 일반 expression context의 `values`는 무엇으로 변환되는가?
3. `values`와 `&values[0]`은 무엇을 가리키는가?
4. array object 자체가 pointer object인가?
5. 대표적인 conversion 예외 context는?

## 12. 핵심 정리
배열은 pointer가 아니며 array expression이 특정 context에서 first element pointer로 변환된다.

## 13. 다음 Step
[Step 15-2. 배열 객체와 포인터 변수의 차이](15-2-array-object-pointer-variable.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.3.2
- [cppreference: Array-to-pointer conversion](https://en.cppreference.com/w/c/language/conversion.html#Array-to-pointer_conversion)
