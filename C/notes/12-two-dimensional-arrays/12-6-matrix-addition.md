# 12-6. 행렬 덧셈

같은 rows와 columns를 가진 두 matrix는 같은 위치의 elements를 더해 결과 matrix를 만들 수 있다.

## 1. 학습 목표
- 두 입력 matrix의 같은 위치를 대응한다.
- nested loop로 결과 matrix의 모든 elements를 계산한다.
- 크기 일치 전제와 signed overflow를 확인한다.

## 2. 선수 지식
Part 7의 덧셈과 Step 12-3의 nested traversal을 사용한다.

## 3. 핵심 개념
`result[row][column] = left[row][column] + right[row][column]`이다. 세 matrix는 같은 2x3 shape를 사용한다. 고정 선언이 이 전제를 만족하며 작은 값으로 signed addition이 `int` 범위 안에 있도록 한다.

## 4. 문법
```c
for (int row = 0; row < ROWS; ++row) {
    for (int column = 0; column < COLUMNS; ++column) {
        result[row][column] =
            left[row][column] + right[row][column];
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int left[2][3] = {{1, 2, 3}, {4, 5, 6}};
    int right[2][3] = {{6, 5, 4}, {3, 2, 1}};
    int result[2][3] = {0};

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 3; ++column) {
            result[row][column] =
                left[row][column] + right[row][column];
        }
    }

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 3; ++column) {
            printf("%d ", result[row][column]);
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
각 위치에서 1+6, 2+5처럼 대응 elements를 더한다. 여섯 결과가 모두 7이 되어 두 rows에 `7 7 7`이 출력된다.

## 7. 내부 동작
[C17 표준] matrix addition은 C의 built-in 전체 배열 연산이 아니라 각 `int` element에 대한 덧셈과 assignment다. signed 합이 표현 범위를 넘으면 Undefined Behavior지만 현재 작은 값은 안전하다. `result = left + right` 같은 배열 전체 expression은 사용할 수 없다.

## 8. 자주 하는 실수
- shape가 다른 matrix를 같은 index로 처리한다.
- row와 column bounds를 뒤바꾼다.
- 결과 matrix를 초기화·assignment하지 않고 읽는다.
- C가 matrix 전체 덧셈 연산자를 제공한다고 생각한다.

## 9. 필수 실습
두 2x3 matrix를 element-wise로 더해 표 형태 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-6/README.md)

## 10. 추가 실습
- ★ 2x2 덧셈
- ★★ 음수를 포함한 덧셈
- ★★★ 각 row의 결과 합 계산

## 11. 확인 문제
1. 두 matrix에 필요한 shape 관계는?
2. 한 result element는 어떻게 계산하는가?
3. 전체 계산 횟수는?
4. C에 배열 전체 `+` 연산이 있는가?
5. signed 덧셈에서 확인할 위험은?

## 12. 핵심 정리
같은 shape의 matrix를 nested loop로 순회하며 대응 elements에 scalar 덧셈과 assignment를 적용한다.

## 13. 다음 Step
[Step 12-7. 전치 행렬](12-7-transpose.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6, 6.5.16
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
