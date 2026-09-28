# 29-4. Selection Sort
## 1. 학습 목표
- Selection Sort의 sorted prefix invariant를 설명한다.
- minimum index 탐색과 swap bounds를 검증한다.
- stability를 실제 구현 기준으로 판단한다.
## 2. 선수 지식
29-2의 탐색과 29-3의 in-place sorting을 안다.
## 3. 핵심 개념
**[Algorithm]** 매 pass에서 unsorted suffix의 최소 원소 index를 찾고 prefix의 다음 위치와 교환한다.

반복 시작 시점:

```text
[0, start)는 최종 위치의 가장 작은 원소들로 정렬되어 있다.
```

suffix minimum을 `start`에 놓으면 invariant가 한 칸 확장된다.
## 4. 문법
```c
for (size_t start = 0; start < count; ++start) {
    size_t min_index = start;

    for (size_t i = start + 1; i < count; ++i) {
        if (values[i] < values[min_index]) {
            min_index = i;
        }
    }
    if (min_index != start) {
        int tmp = values[start];
        values[start] = values[min_index];
        values[min_index] = tmp;
    }
}
```
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static void selection_sort(int values[], size_t count)
{
    for (size_t start = 0; start < count; ++start) {
        size_t min_index = start;

        for (size_t i = start + 1; i < count; ++i) {
            if (values[i] < values[min_index]) {
                min_index = i;
            }
        }
        if (min_index != start) {
            int tmp = values[start];
            values[start] = values[min_index];
            values[min_index] = tmp;
        }
    }
}

int main(void)
{
    int values[] = {4, 2, 7, 2, 1};
    size_t count = sizeof values / sizeof values[0];

    selection_sort(values, count);
    for (size_t i = 0; i < count; ++i) {
        printf("%d%c", values[i], i + 1 == count ? '\n' : ' ');
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o selection_sort
./selection_sort
```
## 6. 코드 해석
`start < count`일 때만 `min_index = start`를 만든다. suffix scan도 `i < count`를 유지한다. 출력은 `1 2 2 4 7`이다.
## 7. 내부 동작
**[Algorithm]** comparison 수는 입력 순서와 무관하게 이 구현에서 대략 `n(n-1)/2`이고 time은 `O(n²)`이다. auxiliary space는 `O(1)`이다.

일반적인 먼 거리 swap은 equal-key 원소의 상대 순서를 바꿀 수 있으므로 이 구현을 stable이라고 보장하지 않는다. `int` 값만 출력하면 그 차이가 보이지 않을 수 있다.

**[C17]** 입력 배열은 mutable하며 함수가 원소 순서를 바꾼다. empty range에서는 body가 실행되지 않는다.
## 8. 자주 하는 실수
- empty input에서 무조건 `values[0]`을 minimum으로 잡는다.
- suffix가 아니라 전체 배열에서 매번 minimum을 찾는다.
- integer output만 보고 stability를 단정한다.
- `start + 1`을 loop guard 밖에서 dereference한다.
## 9. 필수 실습
pass별 `start`, `min_index`, 배열 상태를 표로 기록하고 boundary inputs를 검증한다.
[29-4 exercise](../../exercises/29-basic-algorithms/29-4/README.md)
## 10. 추가 실습
- ★ swap 횟수를 기록한다.
- ★★ descending order로 maximum을 선택한다.
- ★★★ equal key와 original position을 함께 저장해 stability를 관찰한다.
## 11. 확인 문제
1. sorted prefix invariant는 무엇인가?
2. empty input에서 `values[0]`을 읽지 않는 이유는?
3. comparison 수가 입력 순서에 크게 달라지지 않는 이유는?
4. 이 구현의 time과 auxiliary space는?
5. 일반적인 Selection Sort가 unstable할 수 있는 이유는?
## 12. 핵심 정리
- suffix minimum을 골라 sorted prefix를 확장한다.
- 모든 index는 `[0, count)` 안에서만 사용한다.
- stability는 algorithm 이름이 아니라 실제 이동 규칙으로 판단한다.
## 13. 다음 Step
[29-5. Insertion Sort](29-5-insertion-sort.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — selection sort.
- N1570 6.5.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 배열 규칙은 C17에서도 유지된다.
