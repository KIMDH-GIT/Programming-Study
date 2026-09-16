# 11-5. `sizeof(array)`와 원소 수

실제 array object에 `sizeof`를 적용하면 전체 배열의 C byte 수를 얻고, 한 element 크기로 나누면 element count를 계산할 수 있다.

## 1. 학습 목표
- 전체 배열 byte 수와 element byte 수를 구분한다.
- `sizeof(array) / sizeof(array[0])`로 count를 계산한다.
- `sizeof` 결과형 `size_t`와 `%zu`를 사용한다.

## 2. 선수 지식
Part 2의 `sizeof`·`size_t`·C byte와 Step 11-4의 연속 element를 안다.

## 3. 핵심 개념
`sizeof(values)`는 이 문맥에서 실제 array object 전체의 크기다. `sizeof(values[0])`은 첫 element 하나의 크기다. 두 값의 비율이 element count다. element count 5와 전체 byte 수는 같은 개념이 아니며 `sizeof(int)`는 구현에 따라 다를 수 있다.

## 4. 문법
```c
size_t bytes = sizeof(values);
size_t element_bytes = sizeof(values[0]);
size_t count = sizeof(values) / sizeof(values[0]);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    size_t count = sizeof(values) / sizeof(values[0]);

    printf("array bytes: %zu\n", sizeof(values));
    printf("element bytes: %zu\n", sizeof(values[0]));
    printf("element count: %zu\n", count);
    return 0;
}
```

## 6. 코드 해석
전체 byte 수는 `5 * sizeof(int)`이고 element byte 수는 `sizeof(int)`다. 나누면 구현의 `int` 크기와 무관하게 count 5를 얻는다. 출력 숫자 중 byte 수는 환경에 따라 달라질 수 있지만 count는 5다.

## 7. 내부 동작
[C17 표준] `sizeof` 결과형은 unsigned integer type인 `size_t`다. `sizeof`의 단위는 C byte이며 `CHAR_BIT` bits다. 이 count 식은 현재 scope의 실제 array object에 적용할 때 의미가 유지된다. 함수 parameter의 배열 표기는 별도 규칙이 있으므로 Part 15~16에서 다룬다.

## 8. 자주 하는 실수
- `sizeof(values)`를 element count라고 생각한다.
- `int`가 항상 4 byte이므로 전체가 항상 20이라고 단정한다.
- `size_t`를 `%d`로 출력한다.
- array parameter에도 같은 식이 그대로 count를 준다고 일반화한다.

## 9. 필수 실습
서로 다른 element count의 두 배열에서 전체 byte·element byte·count를 출력한다. [실습 README](../../exercises/11-arrays/11-5/README.md)

## 10. 추가 실습
- ★ `double` 배열 count 계산
- ★★ count를 반복 조건으로 사용
- ★★★ element count와 byte 수 비교표 작성

## 11. 확인 문제
1. `sizeof(values)`는 무엇을 반환하는가?
2. `sizeof(values[0])`은 무엇의 크기인가?
3. `sizeof` 결과형은?
4. `size_t` 출력 서식은?
5. element count와 byte 수가 다른 이유는?
6. array parameter에 count 식을 그대로 일반화하면 안 되는 이유는?

## 12. 핵심 정리
실제 배열의 전체 크기를 element 하나의 크기로 나누면 count를 얻으며, byte 수와 count를 구분하고 `size_t`를 사용한다.

## 13. 다음 Step
[Step 11-6. 배열 범위 밖 접근과 Undefined Behavior](11-6-out-of-bounds-ub.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.4, 7.19
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof.html)
- [cppreference: size_t](https://en.cppreference.com/w/c/types/size_t.html)
