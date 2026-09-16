# 12-8. 행렬 곱셈

matrix multiplication은 left의 한 row와 right의 한 column에서 대응 값을 곱해 누적하여 result element 하나를 만든다.

## 1. 학습 목표
- 곱셈 가능한 matrix dimensions를 확인한다.
- 세 nested loops의 row·column·shared index 역할을 구분한다.
- 각 result element의 곱-누적 과정을 추적한다.

## 2. 선수 지식
Part 9의 nested loop와 누적, Step 12-3의 두 index, Step 12-6의 result matrix를 사용한다.

## 3. 핵심 개념
left가 2x3이고 right가 3x2이면 left columns 3과 right rows 3이 같아 곱셈할 수 있고 result는 2x2다. `result[row][column]`은 `left[row][shared] * right[shared][column]`을 shared 0~2에서 모두 더한 값이다.

## 4. 문법
```c
for (int row = 0; row < LEFT_ROWS; ++row) {
    for (int column = 0; column < RIGHT_COLUMNS; ++column) {
        int sum = 0;
        for (int shared = 0; shared < LEFT_COLUMNS; ++shared) {
            sum += left[row][shared] * right[shared][column];
        }
        result[row][column] = sum;
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int left[2][3] = {{1, 2, 3}, {4, 5, 6}};
    int right[3][2] = {{7, 8}, {9, 10}, {11, 12}};
    int result[2][2] = {0};

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 2; ++column) {
            int sum = 0;
            for (int shared = 0; shared < 3; ++shared) {
                sum += left[row][shared] * right[shared][column];
            }
            result[row][column] = sum;
        }
    }

    for (int row = 0; row < 2; ++row) {
        for (int column = 0; column < 2; ++column) {
            printf("%d ", result[row][column]);
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
`result[0][0]`은 `1*7 + 2*9 + 3*11 = 58`이다. 같은 방식으로 결과는 첫 row `58 64`, 둘째 row `139 154`다. result element 4개마다 shared loop가 3회 실행된다.

## 7. 내부 동작
[C17 표준] C는 matrix multiplication operator를 제공하지 않으므로 scalar multiplication, signed addition, assignment로 구현한다. 현재 작은 값은 `int` 범위 안이다. 큰 값에서는 곱셈이나 누적의 signed overflow가 Undefined Behavior가 될 수 있다.

## 8. 자주 하는 실수
- left columns와 right rows가 달라도 곱한다고 생각한다.
- result shape를 2x3 또는 3x2로 잘못 정한다.
- `sum = 0`을 shared loop 안에 두어 매번 누적을 지운다.
- right index를 `[column][shared]`로 뒤바꾼다.

## 9. 필수 실습
2x3과 3x2 matrix를 곱해 2x2 result를 출력하고 첫 element 계산을 손으로 검산한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-8/README.md)

## 10. 추가 실습
- ★ 2x2 identity matrix와 곱셈
- ★★ 1x3과 3x1 곱셈
- ★★★ 각 result element의 곱-누적 표 작성

## 11. 확인 문제
1. 곱셈 가능 조건은?
2. 2x3과 3x2의 result shape는?
3. shared loop는 result 하나당 몇 번 실행되는가?
4. `sum`은 어디서 0으로 초기화하는가?
5. 큰 값에서 주의할 C17 위험은?
6. 예제의 `result[0][0]`은?

## 12. 핵심 정리
matrix multiplication은 dimension 조건을 확인하고 result 위치마다 left row와 right column의 곱을 shared dimension 전체에서 누적한다.

## 13. 다음 Step
[Step 12-9. Part 12 종합 복습](12-9-part-12-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5, 6.5.6, 6.5.16
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
