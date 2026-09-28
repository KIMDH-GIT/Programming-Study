# 29-5. Insertion Sort
## 1. 학습 목표
- Insertion Sort의 sorted prefix 관점을 설명한다.
- `size_t` underflow 없는 shift loop를 작성한다.
- equal elements의 처리와 stability를 연결한다.
## 2. 선수 지식
29-3~29-4의 sorting contract와 Part 11의 array index를 안다.
## 3. 핵심 개념
**[Algorithm]** `[0, i)` prefix가 이미 정렬되어 있다고 가정하고 `values[i]`를 알맞은 위치까지 왼쪽으로 삽입한다.

반복 시작 시점:

```text
[0, i)는 ascending order로 정렬되어 있다.
```

큰 원소를 한 칸씩 오른쪽으로 shift한 뒤 빈 위치에 key를 둔다.
## 4. 문법
```c
for (size_t i = 1; i < count; ++i) {
    int key = values[i];
    size_t j = i;

    while (j > 0 && values[j - 1] > key) {
        values[j] = values[j - 1];
        --j;
    }
    values[j] = key;
}
```

`j > 0`을 먼저 검사하므로 `j - 1`은 valid다. `size_t j`에 `j >= 0`을 종료 조건으로 쓰지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static void insertion_sort(int values[], size_t count)
{
    for (size_t i = 1; i < count; ++i) {
        int key = values[i];
        size_t j = i;

        while (j > 0 && values[j - 1] > key) {
            values[j] = values[j - 1];
            --j;
        }
        values[j] = key;
    }
}

int main(void)
{
    int values[] = {4, 2, 7, 2, 1};
    size_t count = sizeof values / sizeof values[0];

    insertion_sort(values, count);
    for (size_t i = 0; i < count; ++i) {
        printf("%d%c", values[i], i + 1 == count ? '\n' : ' ');
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o insertion_sort
./insertion_sort
```
## 6. 코드 해석
`i`는 두 번째 원소부터 시작한다. `j`는 삽입 위치를 향해 감소하지만 0보다 클 때만 `j - 1`을 읽는다. 출력은 `1 2 2 4 7`이다.
## 7. 내부 동작
**[Algorithm]** already sorted input의 comparison은 `O(n)`, reverse order worst case는 `O(n²)`이다. auxiliary space는 `O(1)`이다. equal element에는 `>`가 false라서 앞선 equal element를 넘지 않으므로 이 구현은 stable하다.

**[C17]** short-circuit `&&` 때문에 `j == 0`이면 `values[j - 1]`을 평가하지 않는다. unsigned underflow를 algorithm 종료 수단으로 사용하지 않는다.

**[Compiler / CPU]** shift 횟수는 source 수준 operation count이며 실제 instruction count와 동일하지 않다.
## 8. 자주 하는 실수
- `for (size_t j = i - 1; j >= 0; --j)`를 사용한다.
- key를 저장하기 전에 원소를 덮어쓴다.
- `>= key`로 바꾸고 stability 영향은 설명하지 않는다.
- empty·single input에서 별도 special access를 만든다.
## 9. 필수 실습
정렬됨, 역순, 중복, 빈 입력에서 `i`, `j`, prefix 상태를 추적한다.
[29-5 exercise](../../exercises/29-basic-algorithms/29-5/README.md)
## 10. 추가 실습
- ★ shift 횟수를 세어 본다.
- ★★ descending order로 comparison을 바꾼다.
- ★★★ equal key의 original position으로 stability를 검증한다.
## 11. 확인 문제
1. outer loop 시작 시 sorted range는?
2. `j > 0`을 먼저 검사해야 하는 이유는?
3. `>`와 `>=`가 stability에 어떤 차이를 만드는가?
4. best·worst time은 각각 무엇인가?
5. reverse input에서 shift가 많은 이유는?
## 12. 핵심 정리
- sorted prefix에 key를 삽입해 범위를 확장한다.
- unsigned index는 0 검사 뒤 감소시킨다.
- stability는 equal comparison과 이동 규칙으로 판단한다.
## 13. 다음 Step
[29-6. Binary Search](29-6-binary-search.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — insertion sort.
- N1570 6.5.13, 6.5.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 short-circuit·배열 규칙은 C17에서도 유지된다.
