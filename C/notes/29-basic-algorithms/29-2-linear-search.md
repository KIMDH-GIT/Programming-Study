# 29-2. Linear Search
## 1. 학습 목표
- linear search의 입력·출력 contract를 정의한다.
- first occurrence를 반환하는 loop invariant를 설명한다.
- 빈 입력, 중복, 미발견을 bounds 안에서 검증한다.
## 2. 선수 지식
29-1의 contract와 Part 11의 배열 순회를 안다.
## 3. 핵심 개념
**[Algorithm]** Linear Search는 앞에서부터 원소를 하나씩 비교한다. 이 Step의 contract는 중복 target이 있으면 가장 작은 index, 즉 first occurrence를 반환한다.

반복 시작 시점의 invariant:

```text
0 .. i-1 범위에는 target이 없다.
```

`values[i] == target`이면 앞 범위에 target이 없으므로 `i`가 first occurrence다. loop가 끝나면 `0 .. count-1` 전체를 검사했으므로 미발견이다.
## 4. 문법
```c
static int linear_search(const int values[], size_t count,
                         int target, size_t *index);
```

성공 여부와 index를 분리한다. `size_t` 함수에서 설명 없이 `return -1;`을 사용하지 않는다. `SIZE_MAX`도 사용한다면 search failure를 뜻하는 application contract를 별도로 정해야 한다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int linear_search(const int values[], size_t count,
                         int target, size_t *index)
{
    if (index == NULL || (values == NULL && count != 0)) {
        return 0;
    }
    for (size_t i = 0; i < count; ++i) {
        if (values[i] == target) {
            *index = i;
            return 1;
        }
    }
    return 0;
}

int main(void)
{
    int values[] = {4, 2, 7, 2};
    size_t index;

    if (linear_search(values, 4, 2, &index)) {
        printf("index=%zu\n", index);
    }
    printf("missing=%d\n", linear_search(values, 4, 9, &index));
    printf("empty=%d\n", linear_search(NULL, 0, 2, &index));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o linear_search
./linear_search
```
## 6. 코드 해석
`i`는 0부터 `count - 1`까지만 도달한다. target 2의 first occurrence는 index 1이다. 미발견과 빈 입력은 0을 반환하며 이때 `index`를 결과로 읽지 않는다.
## 7. 내부 동작
**[Algorithm]** best case는 첫 비교에서 찾는 `O(1)`, worst case는 `n`개를 모두 보는 `O(n)`이다. 여기서 `n`은 원소 수다. average case는 input distribution을 정하지 않았으므로 고정하지 않는다.

**[C17]** `values`는 read-only parameter이며 `count`를 별도로 받는다. function parameter의 `int values[]`는 adjusted pointer type과 관련되므로 function 안의 `sizeof values`로 원소 수를 구하지 않는다.

**[Compiler / CPU]** Big-O의 `O`는 GCC `-O2`와 관계없고 비교 횟수는 ISA instruction 수가 아니다.
## 8. 자주 하는 실수
- `i <= count`로 마지막 iteration에서 OOB를 만든다.
- 중복에서 first occurrence인지 임의 occurrence인지 정하지 않는다.
- 미발견 index를 출력한다.
- `size_t` 반환에 `-1`을 넣고 contract를 생략한다.
## 9. 필수 실습
빈 배열, 첫 위치, 마지막 위치, 중복, 미발견을 explicit test로 검증한다.
[29-2 exercise](../../exercises/29-basic-algorithms/29-2/README.md)
## 10. 추가 실습
- ★ 비교 횟수를 표에 기록한다.
- ★★ last occurrence contract로 변경한다.
- ★★★ 작은 random input을 explicit boundary test 뒤에 보조로 비교한다.
## 11. 확인 문제
1. 이 구현이 반환하는 중복 target 위치는?
2. `count == 0`일 때 loop body가 실행되지 않는 이유는?
3. first occurrence correctness를 설명하는 invariant는?
4. worst-case `n`은 무엇을 뜻하는가?
5. 미발견과 index 반환을 어떻게 분리했는가?
## 12. 핵심 정리
- `[0, count)`를 앞에서부터 검사한다.
- first occurrence와 failure representation을 contract로 정한다.
- 빈 입력도 dereference 없이 defined하게 처리한다.
## 13. 다음 Step
[29-3. Bubble Sort](29-3-bubble-sort.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — sequential search.
- N1570 6.5.2.1, 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
