# 11-4. 연속 메모리 배치와 원소 주소

C 배열의 element는 index 순서대로 중간 빈칸 없이 연속해서 배치된다.

## 1. 학습 목표
- 배열 element의 연속 배치를 설명한다.
- element 주소를 `%p`로 관찰한다.
- C의 배치 보장과 실제 주소값·CPU 동작을 구분한다.

## 2. 선수 지식
Part 5의 `%p`와 `(void *)`, Step 11-3의 index 순회를 사용한다.

## 3. 핵심 개념
`int values[4]`의 네 `int` element는 배열 안에서 순서대로 연속 배치된다. 인접 element 주소의 간격은 해당 구현의 `sizeof(int)`에 대응한다. `int`가 모든 환경에서 4 C byte라고 일반화하지 않는다. 주소 연산과 pointer의 정확한 의미는 Part 14~15에서 배운다.

## 4. 문법
```c
printf("%p\n", (void *)&values[index]);
```
`%p`에는 `void *` argument를 전달한다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[4] = {10, 20, 30, 40};

    for (int i = 0; i < 4; ++i) {
        printf("%d: %p\n", values[i], (void *)&values[i]);
    }
    printf("element size: %zu\n", sizeof(values[0]));
    return 0;
}
```

## 6. 코드 해석
프로그램은 각 값과 element 주소를 index 순서로 출력한다. 실제 숫자 주소는 실행 환경마다 달라질 수 있다. 관찰한 인접 주소 간격과 `sizeof(values[0])`를 비교하되 특정 숫자를 표준 보장으로 일반화하지 않는다.

## 7. 내부 동작
[C17 표준] array type은 연속적으로 할당된 element type 객체들의 집합이다. [OS/ABI 구현] 표시되는 가상 주소, `sizeof(int)`, 정렬은 구현에 따라 달라진다. [CPU 관점] load/store나 cache line은 이번 규칙의 정의가 아니며 필요하지 않다.

## 8. 자주 하는 실수
- 배열은 pointer라고 설명한다.
- 모든 `int` element 주소가 정확히 4씩 증가한다고 일반화한다.
- 한 실행의 주소값이 다음 실행에도 같다고 기대한다.
- `%p`에 `(void *)` 변환 없이 임의 타입 pointer를 전달한다.

## 9. 필수 실습
4개 `int` element의 주소와 `sizeof` 한 element를 출력해 연속 배치를 관찰한다. [실습 README](../../exercises/11-arrays/11-4/README.md)

## 10. 추가 실습
- ★ `char` 배열 element 주소 관찰
- ★★ element 값과 주소를 같은 줄에 출력
- ★★★ C17 보장과 실행 관찰을 표로 구분

## 11. 확인 문제
1. array elements의 배치에 대해 C17이 보장하는 것은?
2. `sizeof(values[0])`은 무엇의 크기인가?
3. `int` 크기를 항상 4라고 할 수 있는가?
4. 실제 주소값은 모든 실행에서 같은가?
5. 배열과 pointer가 동일한 개념인가?

## 12. 핵심 정리
element는 연속 배치되지만 크기와 주소 숫자는 구현에 의존하며, 배열 자체를 pointer라고 부르면 안 된다.

## 13. 다음 Step
[Step 11-5. `sizeof(array)`와 원소 수](11-5-sizeof-element-count.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.3.4, 6.7.6.2
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof.html)
