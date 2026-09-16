# 12-3. 중첩 반복문 순회

외부 loop로 row를 선택하고 내부 loop로 그 row의 columns를 순회하면 모든 element를 정확히 한 번 방문한다.

## 1. 학습 목표
- 외부 row loop와 내부 column loop를 구분한다.
- 전체 접근 횟수를 rows x columns로 계산한다.
- 두 축의 off-by-one을 독립적으로 검토한다.

## 2. 선수 지식
Part 9의 nested loop와 off-by-one, Step 12-1의 두 index를 사용한다.

## 3. 핵심 개념
2x3 matrix에서 row는 0~1, column은 0~2다. 각 row마다 column loop가 0부터 다시 시작해 세 elements를 방문한다. 총 접근은 `2 * 3 = 6`회다. 표 형태 출력에서는 element 뒤 공백을 출력하고 내부 loop가 끝난 뒤 newline을 출력한다.

## 4. 문법
```c
for (int row = 0; row < ROWS; ++row) {
    for (int column = 0; column < COLUMNS; ++column) {
        use(matrix[row][column]);
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

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 3; ++column) {
            printf("%d ", matrix[row][column]);
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
접근 순서는 `(0,0)`, `(0,1)`, `(0,2)`, `(1,0)`, `(1,1)`, `(1,2)`다. 외부 row 1회마다 내부 column 3회가 모두 실행된다.

| row | column | value |
|---:|---:|---:|
| 0 | 0, 1, 2 | 1, 2, 3 |
| 1 | 0, 1, 2 | 4, 5, 6 |

## 7. 내부 동작
[C17 표준] 모든 subscript는 각 차원의 배열 범위 안이어야 한다. `row <= 2`나 `column <= 3`은 범위 밖 element를 접근할 수 있다. newline 위치는 C 배열 규칙이 아니라 원하는 출력 모양을 만드는 I/O 구성이다.

## 8. 자주 하는 실수
- row와 column loop bounds를 바꿔 쓴다.
- 둘 중 한 조건에 `<=`를 사용한다.
- newline을 내부 loop 안에 넣어 element마다 줄을 바꾼다.
- 전체 접근 횟수를 rows + columns로 계산한다.

## 9. 필수 실습
2x3 배열을 nested loop로 표 형태 출력하고 여섯 접근을 추적한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-3/README.md)

## 10. 추가 실습
- ★ 3x2 표 출력
- ★★ 모든 element 합 계산
- ★★★ row·column·값 추적표 작성

## 11. 확인 문제
1. 외부 loop가 담당하는 축은?
2. 내부 loop는 각 row에서 몇 번 실행되는가?
3. 2x3의 전체 접근 횟수는?
4. newline은 어느 loop 뒤에 두는가?
5. `column <= 3`이 위험한 이유는?

## 12. 핵심 정리
row와 column bounds를 각각 지키는 nested loop로 rows x columns elements를 정확히 한 번씩 순회한다.

## 13. 다음 Step
[Step 12-4. row-major 메모리 배치](12-4-row-major-layout.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.8.5
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
