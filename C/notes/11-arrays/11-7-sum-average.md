# 11-7. 합과 평균

배열 순회와 누적 변수를 결합하면 모든 element의 합과 평균을 계산할 수 있다.

## 1. 학습 목표
- 모든 element를 안전하게 누적한다.
- 정수 나눗셈을 피하고 실수 평균을 계산한다.
- 누적 결과의 signed overflow 가능성을 인식한다.

## 2. 선수 지식
Part 6의 형 변환, Part 9의 누적, Step 11-5의 element count를 사용한다.

## 3. 핵심 개념
합은 0으로 초기화하고 index 0부터 count 미만까지 각 element를 더한다. 평균은 합을 element count로 나눈 값이다. 실수 평균이 필요하면 나눗셈 전에 한 operand를 `double`로 변환한다. 교육용 예제는 합이 `int` 범위 안인 작은 값을 사용한다.

## 4. 문법
```c
int sum = 0;
for (size_t i = 0; i < count; ++i) {
    sum += values[i];
}
double average = (double)sum / (double)count;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    size_t count = sizeof(values) / sizeof(values[0]);
    int sum = 0;

    for (size_t i = 0; i < count; ++i) {
        sum += values[i];
    }

    double average = (double)sum / (double)count;
    printf("sum: %d\n", sum);
    printf("average: %.1f\n", average);
    return 0;
}
```

## 6. 코드 해석
누적값은 10, 30, 60, 100, 150으로 변한다. count는 5이며 두 operand를 `double` 계산에 참여시켜 평균 30.0을 얻는다.

## 7. 내부 동작
[C17 표준] signed `int` 합이 표현 범위를 넘으면 Undefined Behavior다. 현재 합 150은 안전하다. cast는 나눗셈 전에 적용되어 floating arithmetic을 선택한다. count가 0이면 나눗셈 문제가 생기지만 이 선언의 배열은 element 5개를 가진다.

## 8. 자주 하는 실수
- `sum`을 초기화하지 않는다.
- 반복 조건을 `i <= count`로 쓴다.
- 정수끼리 먼저 나눈 뒤 결과를 `double`에 저장한다.
- 큰 element의 합도 `int`에서 항상 안전하다고 가정한다.

## 9. 필수 실습
5개 작은 정수의 합과 실수 평균을 계산한다. [실습 README](../../exercises/11-arrays/11-7/README.md)

## 10. 추가 실습
- ★ 세 값의 합
- ★★ 음수를 포함한 평균
- ★★★ 각 반복의 누적값 표 작성

## 11. 확인 문제
1. 합의 초기값은?
2. 평균 전에 cast가 필요한 이유는?
3. 예제의 count는?
4. signed 합이 범위를 넘으면 어떤 분류인가?
5. `i <= count`가 위험한 이유는?

## 12. 핵심 정리
유효 index 전체를 누적하고 나눗셈 전에 타입을 선택하며, 합과 제수의 표현 범위를 확인한다.

## 13. 다음 Step
[Step 11-8. 최댓값과 최솟값](11-8-min-max.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.4, 6.5.5, 6.5.6
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
