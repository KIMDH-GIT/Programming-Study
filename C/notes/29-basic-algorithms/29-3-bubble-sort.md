# 29-3. Bubble Sort
## 1. 학습 목표
- Bubble Sort의 pass invariant와 loop bounds를 설명한다.
- in-place, stable, worst-case complexity를 구현 기준으로 판단한다.
- swap 없는 pass의 early exit를 검증한다.
## 2. 선수 지식
29-1의 contract, Part 9의 nested loop, Part 11의 배열을 안다.
## 3. 핵심 개념
**[Algorithm]** 인접한 두 원소가 역순이면 교환한다. 한 pass가 끝나면 아직 확정되지 않은 범위의 가장 큰 원소가 그 범위의 끝에 놓인다.

이 구현은 같은 값에 `>`만 사용하므로 equal elements를 교환하지 않는다. key와 원래 위치를 함께 관찰하면 stable임을 확인할 수 있다. 입력 배열을 직접 수정하고 입력 크기에 비례하는 별도 저장 공간을 쓰지 않으므로 이 convention에서는 in-place다.
## 4. 문법
```c
for (size_t end = count; end > 1; --end) {
    int swapped = 0;
    for (size_t i = 1; i < end; ++i) {
        if (values[i - 1] > values[i]) {
            /* ordinary three-assignment swap */
            swapped = 1;
        }
    }
    if (!swapped) {
        break;
    }
}
```

`end > 1`과 `i < end`가 유효한 adjacent pair만 만든다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static void bubble_sort(int values[], size_t count)
{
    for (size_t end = count; end > 1; --end) {
        int swapped = 0;

        for (size_t i = 1; i < end; ++i) {
            if (values[i - 1] > values[i]) {
                int tmp = values[i - 1];
                values[i - 1] = values[i];
                values[i] = tmp;
                swapped = 1;
            }
        }
        if (!swapped) {
            break;
        }
    }
}

int main(void)
{
    int values[] = {4, 2, 7, 2, 1};
    size_t count = sizeof values / sizeof values[0];

    bubble_sort(values, count);
    for (size_t i = 0; i < count; ++i) {
        printf("%d%c", values[i], i + 1 == count ? '\n' : ' ');
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bubble_sort
./bubble_sort
```
## 6. 코드 해석
각 inner loop는 `values[i - 1]`과 `values[i]`만 읽는다. `i`는 1부터 `end - 1`까지이므로 두 index 모두 valid다. 출력은 `1 2 2 4 7`이다.
## 7. 내부 동작
**[Algorithm]** pass 뒤 suffix 하나가 확정된다. worst case time은 `O(n²)`, 이미 정렬된 입력은 early exit로 `O(n)` 비교다. auxiliary space는 `O(1)`이다. `n`은 원소 수다.

**[C17]** `count == 0`이나 1이면 outer loop가 실행되지 않는다. `size_t` reverse traversal을 `i >= 0`으로 작성하지 않는다.

**[Compiler / CPU]** comparison·swap 수는 source algorithm 분석이며 machine instruction 수와 동일하지 않다.
## 8. 자주 하는 실수
- `i < end` 대신 `i <= end`로 OOB를 만든다.
- pass 뒤 어떤 suffix가 확정되는지 설명 없이 bounds를 외운다.
- swap이 없는데도 무조건 모든 pass를 돈다.
- 모든 sort가 stable이라고 일반화한다.
## 9. 필수 실습
빈 배열, 원소 하나, 정렬됨, 역순, 중복 입력을 검증하고 pass별 배열 상태를 기록한다.
[29-3 exercise](../../exercises/29-basic-algorithms/29-3/README.md)
## 10. 추가 실습
- ★ early exit 횟수를 기록한다.
- ★★ descending order로 comparison을 바꾼다.
- ★★★ `(key, original_position)` pair로 stability를 추적한다.
## 11. 확인 문제
1. 한 pass 뒤 어떤 원소가 확정되는가?
2. `end > 1`이 빈 입력에서 안전한 이유는?
3. 이 구현이 stable인 comparison 조건은?
4. worst-case time과 auxiliary space는?
5. in-place가 추가 변수 0개라는 뜻이 아닌 이유는?
## 12. 핵심 정리
- adjacent inversion을 교환해 큰 값을 suffix로 보낸다.
- loop bounds는 valid adjacent pair를 기준으로 세운다.
- early exit는 swap 없는 pass에서만 적용한다.
## 13. 다음 Step
[29-4. Selection Sort](29-4-selection-sort.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — bubble sort.
- N1570 6.5.2.1. N1570은 **C11 공개 Committee Draft**이며 배열 access 규칙은 C17에서도 유지된다.
