# 12-5. `sizeof`로 행·원소 크기 확인

실제 2차원 array object에서 `sizeof`는 전체 matrix, 한 row, 한 scalar element의 C byte 수를 단계별로 보여 준다.

## 1. 학습 목표
- `sizeof(matrix)`, `sizeof(matrix[0])`, `sizeof(matrix[0][0])`를 구분한다.
- row count와 column count를 계산한다.
- `size_t`와 `%zu`를 정확히 사용한다.

## 2. 선수 지식
Part 11의 `sizeof(array)` count 계산과 Step 12-1의 array-of-arrays를 사용한다.

## 3. 핵심 개념
2x3 `int` matrix에서 전체는 row 2개, 한 row는 `int` 3개, scalar element는 `int` 하나다. 전체 크기/row 크기는 row count 2, row 크기/element 크기는 column count 3을 준다. byte 수는 구현의 `sizeof(int)`에 따라 달라지지만 counts는 선언대로다.

## 4. 문법
```c
size_t rows = sizeof(matrix) / sizeof(matrix[0]);
size_t columns = sizeof(matrix[0]) / sizeof(matrix[0][0]);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int matrix[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };
    size_t rows = sizeof(matrix) / sizeof(matrix[0]);
    size_t columns = sizeof(matrix[0]) / sizeof(matrix[0][0]);

    printf("matrix bytes: %zu\n", sizeof(matrix));
    printf("row bytes: %zu\n", sizeof(matrix[0]));
    printf("element bytes: %zu\n", sizeof(matrix[0][0]));
    printf("rows: %zu, columns: %zu\n", rows, columns);
    return 0;
}
```

## 6. 코드 해석
`sizeof(matrix)`는 전체 6개 `int`, `sizeof(matrix[0])`은 한 row의 3개 `int`, `sizeof(matrix[0][0])`은 `int` 하나의 크기다. 계산 결과 rows 2, columns 3이다.

## 7. 내부 동작
[C17 표준] `sizeof` 결과형은 `size_t`이고 단위는 C byte다. 이 계산은 현재 scope의 실제 matrix object에서 수행된다. 함수 parameter의 array 표기는 adjustment 규칙이 있으므로 같은 식을 무조건 일반화하지 않고 Part 15~16에서 다룬다.

## 8. 자주 하는 실수
- 전체 matrix 크기를 element count 6이라고 부른다.
- row 크기를 row count라고 생각한다.
- `size_t`를 `%d`로 출력한다.
- `sizeof(int)`가 항상 4라고 가정한다.

## 9. 필수 실습
2x3 배열의 전체·row·element byte 수와 row·column counts를 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-5/README.md)

## 10. 추가 실습
- ★ 3x2 배열 counts
- ★★ `double` 2x2 배열 크기
- ★★★ 세 `sizeof` 관계를 식으로 설명

## 11. 확인 문제
1. `sizeof(matrix[0])`은 무엇의 크기인가?
2. column count 식은?
3. row count 식은?
4. `sizeof` 결과형과 출력 서식은?
5. `int`가 항상 4 C byte인가?
6. 함수 parameter에도 같은 계산을 무조건 써도 되는가?

## 12. 핵심 정리
전체 matrix, row array, scalar element의 `sizeof`를 나누어 row와 column counts를 얻고 byte 수와 count를 구분한다.

## 13. 다음 Step
[Step 12-6. 행렬 덧셈](12-6-matrix-addition.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.4, 7.19
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof.html)
- [cppreference: size_t](https://en.cppreference.com/w/c/types/size_t.html)
