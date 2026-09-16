# 12-4. row-major 메모리 배치

C의 2차원 배열은 각 row의 elements가 연속되고, row 배열들도 index 순서대로 연속되는 array-of-arrays 구조다.

## 1. 학습 목표
- row-major element 순서를 나열한다.
- C array type의 배치 규칙과 CPU cache를 구분한다.
- row 전체 assignment의 제약을 이해한다.

## 2. 선수 지식
Part 11의 연속 array element와 Step 12-1의 row array 구조를 안다.

## 3. 핵심 개념
`int matrix[2][3]`의 element 순서는 `(0,0)`, `(0,1)`, `(0,2)`, `(1,0)`, `(1,1)`, `(1,2)`다. 각 row는 3개의 `int` 배열이고 두 row도 상위 배열의 elements라 순서대로 연속 배치된다. 이 배열-안의-배열 배치를 흔히 row-major order라고 부른다.

## 4. 문법
```c
int matrix[2][3];
/* matrix[0] 다음에 matrix[1]이 배열 element 순서로 배치 */
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

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 3; ++column) {
            printf("(%d,%d)=%d\n", row, column, matrix[row][column]);
        }
    }
    return 0;
}
```

## 6. 코드 해석
출력은 row 0의 세 values 후 row 1의 세 values 순서다. 이 순서가 선언된 array-of-arrays의 element 순서와 일치한다.

## 7. 내부 동작
[C17 표준] array elements는 순서대로 연속 배치되므로 row 내부와 row 배열 사이 모두 array 규칙이 적용된다. [CPU 구현] cache line이나 물리 memory는 C17이 보장하지 않는다. row 순회가 많은 실제 시스템에서 locality에 유리할 수 있지만 row-major 자체를 CPU 규칙으로 설명하면 안 된다.

## 8. 자주 하는 실수
- CPU가 row-major라서 C가 이 순서를 쓴다고 설명한다.
- 2차원 배열을 평평한 pointer나 `int **`라고 부른다.
- `matrix[0] = matrix[1];`로 row 전체 assignment가 된다고 생각한다.
- row 내부 연속성과 행 사이 배치를 별개라고 생각한다.

## 9. 필수 실습
2x3 배열을 row-major index 순서와 함께 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-4/README.md)

## 10. 추가 실습
- ★ 3x2 순서 나열
- ★★ column을 바깥 loop로 바꾼 출력 순서 비교
- ★★★ C17 배치 규칙과 cache 설명을 구분한 표 작성

## 11. 확인 문제
1. 2x3 배열의 네 번째 element 위치는?
2. 각 row의 type은?
3. row-major는 CPU 명령 규칙인가?
4. row 배열들도 연속 배치되는 이유는?
5. `matrix[0] = matrix[1]`이 허용되는가?

## 12. 핵심 정리
row-major는 array-of-arrays의 연속 element 배치에서 나오며 CPU cache는 별도의 구현 관점이다.

## 13. 다음 Step
[Step 12-5. `sizeof`로 행·원소 크기 확인](12-5-sizeof-rows-elements.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.7.6.2
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
