# 11-8. 최댓값과 최솟값

배열의 첫 element를 초기 후보로 두고 나머지 element와 비교하면 최댓값과 최솟값을 찾을 수 있다.

## 1. 학습 목표
- 첫 element로 초기 후보를 설정한다.
- 두 번째 element부터 끝까지 비교한다.
- 배열이 비어 있지 않다는 전제를 명시한다.

## 2. 선수 지식
Part 8의 비교 조건과 Step 11-3의 배열 순회를 사용한다.

## 3. 핵심 개념
임의의 정수 배열에서는 0을 초기 최댓값으로 쓰면 모든 값이 음수일 때 틀릴 수 있다. 따라서 element가 최소 하나 있다는 전제에서 `max = values[0]`, `min = values[0]`으로 시작한다. index 1부터 각 값이 후보보다 크거나 작은지 비교한다.

## 4. 문법
```c
int maximum = values[0];
int minimum = values[0];
for (size_t i = 1; i < count; ++i) {
    if (values[i] > maximum) { maximum = values[i]; }
    if (values[i] < minimum) { minimum = values[i]; }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {-3, 7, 2, -8, 4};
    size_t count = sizeof(values) / sizeof(values[0]);
    int maximum = values[0];
    int minimum = values[0];

    for (size_t i = 1; i < count; ++i) {
        if (values[i] > maximum) {
            maximum = values[i];
        }
        if (values[i] < minimum) {
            minimum = values[i];
        }
    }

    printf("max: %d\n", maximum);
    printf("min: %d\n", minimum);
    return 0;
}
```

## 6. 코드 해석
초기 후보는 -3이다. 7에서 maximum이 7이 되고 -8에서 minimum이 -8이 된다. 나머지는 후보를 바꾸지 않아 최종 결과는 7과 -8이다.

## 7. 내부 동작
[C17 표준] `values[0]`을 읽으려면 배열에 최소 한 element가 있어야 한다. 이 예제의 array declarator는 5개를 명시해 전제를 만족한다. 표준 C의 일반 배열을 element count 0으로 선언하는 것은 허용되지 않으며 compiler extension과 구분한다.

## 8. 자주 하는 실수
- maximum과 minimum을 무조건 0으로 초기화한다.
- count가 0일 수 있는데 `values[0]`을 읽는다.
- index 0을 이미 후보로 썼는데 잘못된 범위로 다시 접근한다.
- 비교 방향을 반대로 쓴다.

## 9. 필수 실습
음수와 양수가 섞인 5개 배열에서 최댓값과 최솟값을 구한다. [실습 README](../../exercises/11-arrays/11-8/README.md)

## 10. 추가 실습
- ★ 모두 양수인 배열
- ★★ 모두 음수인 배열
- ★★★ 후보가 바뀌는 index 기록

## 11. 확인 문제
1. 첫 element를 후보로 쓰는 이유는?
2. 반복을 index 1에서 시작하는 이유는?
3. 모두 음수일 때 maximum을 0으로 시작하면 왜 틀리는가?
4. 이 알고리즘의 필수 count 전제는?
5. 예제의 최댓값과 최솟값은?

## 12. 핵심 정리
비어 있지 않은 배열의 첫 element를 후보로 삼고 나머지를 비교하면 값의 부호와 무관하게 최댓값·최솟값을 찾는다.

## 13. 다음 Step
[Step 11-9. 역순 출력](11-9-reverse-output.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.8
- [cppreference: Relational operators](https://en.cppreference.com/w/c/language/operator_comparison.html)
