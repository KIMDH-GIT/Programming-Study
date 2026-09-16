# 12-9. Part 12 종합 복습

Part 12에서는 array-of-arrays 선언·초기화·순회·row-major 배치·크기와 matrix 덧셈·transpose·곱셈을 학습했다.

## 1. 학습 목표
- row·column index와 shape를 종합한다.
- C17 array layout, `sizeof`, 경계 규칙을 적용한다.
- matrix 연산을 scalar element 연산으로 구성한다.

## 2. 선수 지식
Step 12-1부터 12-8까지와 Part 9~11의 nested loop·array·UB 규칙을 사용한다.

## 3. 핵심 개념
`int matrix[R][C]`는 `int[C]` row를 R개 가진 배열이다. 유효 index는 row 0~R-1, column 0~C-1이다. rows와 columns를 nested loop로 순회하며 row-major 순서는 C의 array-of-arrays 배치에서 나온다. 2차원 배열은 pointer나 `int **`가 아니다.

## 4. 문법
```c
size_t rows = sizeof(matrix) / sizeof(matrix[0]);
size_t columns = sizeof(matrix[0]) / sizeof(matrix[0][0]);
for (size_t row = 0; row < rows; ++row) {
    for (size_t column = 0; column < columns; ++column) {
        /* matrix[row][column] */
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
    size_t rows = sizeof(matrix) / sizeof(matrix[0]);
    size_t columns = sizeof(matrix[0]) / sizeof(matrix[0][0]);
    int sum = 0;

    for (size_t row = 0; row < rows; ++row) {
        for (size_t column = 0; column < columns; ++column) {
            sum += matrix[row][column];
            printf("%d ", matrix[row][column]);
        }
        printf("\n");
    }

    printf("sum: %d\n", sum);
    return 0;
}
```

## 6. 코드 해석
rows는 2, columns는 3이다. 여섯 elements를 row-major 순서로 표처럼 출력하고 작은 값들을 한 번씩 누적해 합 21을 만든다.

## 7. 내부 동작
[C17 표준] 두 차원의 모든 subscript가 유효해야 한다. `matrix[rows][0]`이나 `matrix[0][columns]` 접근은 Undefined Behavior이며 항상 segmentation fault라고 예측할 수 없다. 전체 matrix나 row array는 scalar처럼 assignment할 수 없다. 함수 parameter adjustment와 pointer-to-array는 후속 Part에서 다룬다.

## 8. 자주 하는 실수
- rows와 columns를 바꾸어 경계를 설정한다.
- 2차원 배열을 `int **`라고 설명한다.
- row 전체 또는 matrix 전체 assignment를 시도한다.
- automatic matrix를 초기화하지 않고 읽는다.
- C17 row-major 배치와 CPU cache를 같은 규칙으로 설명한다.

## 9. 필수 실습
2x3 matrix의 rows·columns, 표 출력, 전체 합을 한 프로그램에서 검증한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-9/README.md)

## 10. 추가 실습
- ★ 각 row의 합
- ★★ 각 column의 합
- ★★★ target의 found row·column을 nested loop로 찾기

## 11. 확인 문제
1. `int matrix[2][3]`의 element type은?
2. 유효 row와 column 범위는?
3. row count와 column count 식은?
4. row-major 순서의 네 번째 위치는?
5. 2차원 배열과 `int **`가 같은가?
6. `matrix[2][0]` 접근의 C17 분류는?
7. 2x3과 3x2 multiplication 결과 shape는?

## 12. 핵심 정리
2차원 배열은 연속된 row arrays이며 각 차원의 shape와 경계를 지켜 nested loops와 scalar 연산으로 처리한다.

## 13. 다음 Step
Step 13-1. `char`와 문자 `'A'`

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.2.1, 6.5.3.4, 6.7.6.2, 6.7.9
- [cppreference: Arrays](https://en.cppreference.com/w/c/language/array.html)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
