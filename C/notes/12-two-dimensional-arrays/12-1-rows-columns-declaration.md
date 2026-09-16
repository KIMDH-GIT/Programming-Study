# 12-1. 행·열과 2차원 배열 선언

2차원 배열은 C에서 별도의 matrix primitive가 아니라 element가 다시 배열인 **array of arrays**다.

## 1. 학습 목표
- `int matrix[3][4]`를 row와 column 구조로 해석한다.
- 두 subscript의 역할과 유효 범위를 구분한다.
- 2차원 배열과 pointer-to-pointer를 동일시하지 않는다.

## 2. 선수 지식
Part 11의 array type, element, index, 연속 배치와 경계 규칙을 사용한다.

## 3. 핵심 개념
`int matrix[3][4]`는 3개의 element를 가진 배열이고, 각 element는 다시 4개의 `int`를 가진 배열이다. 초보자 관점에서는 3 rows, 각 row에 4 columns로 볼 수 있다. `matrix`의 element type은 `int[4]`이며 전체 `int` element는 12개다.

## 4. 문법
```c
int matrix[ROWS][COLUMNS];
matrix[row][column]
```

`matrix[1][2]`는 두 번째 row의 세 번째 column element다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int matrix[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };

    printf("%d\n", matrix[0][0]);
    printf("%d\n", matrix[1][2]);
    return 0;
}
```

## 6. 코드 해석
첫 subscript는 row를, 둘째는 그 row 안의 column을 선택한다. `matrix[0][0]`은 1, `matrix[1][2]`는 6이다. 2x3 배열의 row 범위는 0~1, column 범위는 0~2다.

## 7. 내부 동작
[C17 표준] array type의 element는 다시 array type일 수 있다. 각 row는 `int[3]` 객체이고 row 배열도 순서대로 배치된다. `int matrix[2][3]`은 `int **`와 같은 type이 아니며 배열을 pointer라고 설명하면 안 된다. pointer-to-array 관계는 Part 14~16에서 다룬다.

## 8. 자주 하는 실수
- `[2][3]`을 마지막 row 2, column 3이라고 생각한다.
- `matrix[2][0]`이나 `matrix[0][3]`을 유효하다고 생각한다.
- 첫 subscript를 column이라고 읽는다.
- 2차원 배열을 `int **`와 같은 type이라고 설명한다.

## 9. 필수 실습
2 rows, 3 columns 배열을 선언·초기화하고 네 모서리 element를 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-1/README.md)

## 10. 추가 실습
- ★ 3x2 배열 선언
- ★★ 특정 subscript가 가리키는 row·column 설명
- ★★★ 여러 subscript의 유효성 판별

## 11. 확인 문제
1. `int matrix[3][4]`의 row 수와 column 수는?
2. `matrix`의 element type은?
3. `matrix[1][2]`는 몇 번째 row와 column인가?
4. row와 column의 마지막 유효 index는?
5. `int matrix[3][4]`와 `int **`가 같은 type인가?

## 12. 핵심 정리
2차원 배열은 row 배열을 element로 가지는 array of arrays이며 두 index를 각각 유효 범위 안에서 사용한다.

## 13. 다음 Step
[Step 12-2. 2차원 배열 초기화](12-2-initialization.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.2.1, 6.7.6.2
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
