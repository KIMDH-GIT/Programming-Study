# 29-6. Binary Search
## 1. 학습 목표
- sorted-input precondition을 명시한다.
- half-open interval `[begin, end)`를 안전하게 갱신한다.
- overflow와 unsigned underflow를 피한 midpoint를 계산한다.
## 2. 선수 지식
29-2의 search contract와 29-3~29-5의 ascending order를 안다.
## 3. 핵심 개념
**[Algorithm]** Binary Search는 정렬된 범위의 가운데 원소와 target을 비교해 가능한 범위를 절반씩 줄인다.

입력 조건:

```text
values[0 .. count)는 같은 ordering 기준으로 ascending 정렬되어 있다.
```

정렬되지 않은 입력은 반드시 C UB인 것이 아니다. 대신 algorithm precondition 위반이므로 의도한 검색 결과가 보장되지 않는다.
## 4. 문법
```c
size_t begin = 0;
size_t end = count;

while (begin < end) {
    size_t mid = begin + (end - begin) / 2;

    if (values[mid] < target) {
        begin = mid + 1;
    } else if (values[mid] > target) {
        end = mid;
    } else {
        /* found */
    }
}
```

range는 `[begin, end)`이고 element count는 `end - begin`이다. `end = mid`를 사용해 `mid == 0`에서 `mid - 1` unsigned underflow를 만들지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int binary_search(const int values[], size_t count,
                         int target, size_t *index)
{
    size_t begin = 0;
    size_t end = count;

    if (index == NULL || (values == NULL && count != 0)) {
        return 0;
    }
    while (begin < end) {
        size_t mid = begin + (end - begin) / 2;

        if (values[mid] < target) {
            begin = mid + 1;
        } else if (values[mid] > target) {
            end = mid;
        } else {
            *index = mid;
            return 1;
        }
    }
    return 0;
}

int main(void)
{
    int values[] = {1, 3, 5, 7, 9};
    size_t index;

    if (binary_search(values, 5, 7, &index)) {
        printf("index=%zu\n", index);
    }
    printf("missing=%d\n", binary_search(values, 5, 4, &index));
    printf("empty=%d\n", binary_search(NULL, 0, 4, &index));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o binary_search
./binary_search
```
## 6. 코드 해석
midpoint는 현재 range 안에 있다. 작은 target이면 `end = mid`, 큰 target이면 `begin = mid + 1`로 range가 반드시 줄어든다. 출력은 `index=3`, `missing=0`, `empty=0`이다.
## 7. 내부 동작
**[Algorithm]** invariant는 “target이 존재한다면 현재 `[begin, end)` 안에 있다”이다. 매 iteration에서 range 크기가 줄어 종료한다. worst-case time은 `O(log n)`, auxiliary space는 `O(1)`이며 `n`은 원소 수다.

**[C17]** `end - begin`은 invariant `begin <= end` 아래에서 안전하다. `(begin + end) / 2` 대신 difference 기반 식을 써 합의 overflow 가능성을 피한다.

**[Compiler / CPU]** `O(log n)`은 실제 실행 시간이 항상 Linear Search보다 짧다는 뜻이 아니다. 작은 input, cache, branch, compiler, CPU가 실제 시간을 바꾼다.
## 8. 자주 하는 실수
- sorted precondition을 생략한다.
- unsigned `high = mid - 1`에서 `mid == 0`을 놓친다.
- `(low + high) / 2`를 모든 type·range에서 안전하다고 한다.
- 미발견을 UB라고 설명한다.
## 9. 필수 실습
빈 배열, 원소 하나, first, last, middle, absent target을 정렬된 입력에서 검증한다.
[29-6 exercise](../../exercises/29-basic-algorithms/29-6/README.md)
## 10. 추가 실습
- ★ range `[begin, end)`를 iteration별로 기록한다.
- ★★ 중복 target에서 반환 가능한 index를 contract로 정한다.
- ★★★ Linear Search와 comparison 수를 같은 vectors로 비교한다.
## 11. 확인 문제
1. Binary Search의 핵심 precondition은?
2. empty range는 어떻게 표현하는가?
3. midpoint 식이 합 overflow를 피하는 이유는?
4. `end = mid`가 unsigned underflow를 피하는 방법은?
5. `O(log n)`이 실제 시간 비교를 보장하지 않는 이유는?
## 12. 핵심 정리
- ordering precondition을 만족하는 범위에서만 결과를 보장한다.
- half-open interval은 empty range와 count를 자연스럽게 표현한다.
- range는 매 iteration 줄어들고 모든 index는 bounds 안에 있다.
## 13. 다음 Step
[29-7. `my_strlen`](29-7-my-strlen.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — binary search.
- N1570 6.2.5, 6.5.6. N1570은 **C11 공개 Committee Draft**이며 unsigned arithmetic 규칙은 C17에서도 유지된다.
