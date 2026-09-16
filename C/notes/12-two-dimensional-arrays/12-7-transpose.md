# 12-7. 전치 행렬

transpose는 원본의 row와 column 위치를 서로 바꿔 `transposed[column][row] = matrix[row][column]`로 만드는 연산이다.

## 1. 학습 목표
- 원본과 transpose의 shape 관계를 설명한다.
- 두 index를 바꾸어 별도 결과 배열에 저장한다.
- 단순 전치 출력과 실제 결과 배열 생성을 구분한다.

## 2. 선수 지식
Step 12-3의 nested traversal과 Step 12-6의 결과 matrix assignment를 안다.

## 3. 핵심 개념
2x3 matrix의 transpose는 3x2다. 원본 `(row, column)`의 값은 결과 `(column, row)`로 이동한다. 출력할 때 index 순서만 바꿔 보이는 것과, 실제 `transposed` array에 값을 저장하는 것은 다른 작업이다. 이 Step은 별도 array를 생성한다.

## 4. 문법
```c
for (int row = 0; row < ROWS; ++row) {
    for (int column = 0; column < COLUMNS; ++column) {
        transposed[column][row] = matrix[row][column];
    }
}
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
    int transposed[3][2] = {0};

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 3; ++column) {
            transposed[column][row] = matrix[row][column];
        }
    }

    for (int row = 0; row < 3; ++row) {
        for (int column = 0; column < 2; ++column) {
            printf("%d ", transposed[row][column]);
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
원본 row 0의 1,2,3은 결과의 column 0이 되고 원본 row 1의 4,5,6은 결과의 column 1이 된다. 출력은 `1 4`, `2 5`, `3 6`의 3x2 표다.

## 7. 내부 동작
[C17 표준] 두 arrays는 서로 다른 objects이고 각 scalar element에 assignment한다. 2x3 전체 배열을 3x2 배열에 한 번에 대입하는 문법은 없다. 모든 source와 destination subscripts가 각 shape 범위 안에 있어야 한다.

## 8. 자주 하는 실수
- 결과 shape를 원본과 같은 2x3으로 둔다.
- assignment 양쪽 index를 모두 같은 순서로 써 단순 복사한다.
- 출력 index만 바꾼 것을 결과 배열 생성과 같다고 말한다.
- 정사각 matrix만 transpose할 수 있다고 생각한다.

## 9. 필수 실습
2x3 matrix를 별도 3x2 array로 transpose하고 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-7/README.md)

## 10. 추가 실습
- ★ 2x2 transpose
- ★★ 3x2를 2x3으로 transpose
- ★★★ source·destination index 대응표 작성

## 11. 확인 문제
1. 2x3의 transpose shape는?
2. 원본 `(1,2)`는 결과의 어느 위치인가?
3. assignment 식은?
4. 출력 순서만 바꾸는 것과 결과 저장은 같은가?
5. 정사각 matrix만 transpose 가능한가?

## 12. 핵심 정리
transpose는 shape를 columns x rows로 바꾸고 각 source `(row,column)`을 destination `(column,row)`에 저장한다.

## 13. 다음 Step
[Step 12-8. 행렬 곱셈](12-8-matrix-multiplication.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.16
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
