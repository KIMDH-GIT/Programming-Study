# 29-12. 배열·함수·포인터로 알고리즘 통합
## 1. 학습 목표
- array input, count, output pointers를 하나의 contract로 통합한다.
- empty input을 status로 처리한다.
- caller와 algorithm function의 책임을 분리한다.
## 2. 선수 지식
Part 11·15·16의 배열과 포인터, 29-1~29-11의 contract를 안다.
## 3. 핵심 개념
array algorithm은 보통 다음 정보를 분리해 받는다.

```text
values: 첫 원소를 가리키는 pointer
count: 접근 가능한 element 수
output pointers: 성공 시 기록할 결과 object
status: 결과 존재 여부
```

function parameter의 `int values[]`는 array 전체 크기를 보존하지 않는다. caller가 element count를 함께 전달한다.
읽기 쉬운 index notation이 이 범위에 적합하므로 pointer arithmetic만으로 다시 쓰지 않는다. signed/unsigned warning은 무분별한 cast로 숨기지 않고 count와 index의 contract에 맞는 type을 선택해 해결한다.
## 4. 문법
```c
static int min_max(const int values[], size_t count,
                   int *min_value, int *max_value);
```

입력 조건:
- `count > 0`이면 `values`는 호출 동안 읽을 수 있는 `count`개 `int` 원소를 나타낸다.
- 성공 경로에서 output pointers는 호출 동안 살아 있는 서로 다른 writable `int` objects를 가리킨다.
- 두 output object는 input array의 원소와 겹치지 않는다.
- non-NULL pointer의 lifetime·extent·writability와 overlap 조건은 caller precondition이며 이 함수가 검사할 수 없다.

결과 조건:
- 성공하면 두 output에 범위의 minimum·maximum을 기록한다.
- `count == 0`이거나 argument 하나가 NULL이면 0을 반환한다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int min_max(const int values[], size_t count,
                   int *min_value, int *max_value)
{
    if (count == 0 || values == NULL ||
        min_value == NULL || max_value == NULL) {
        return 0;
    }
    *min_value = values[0];
    *max_value = values[0];
    for (size_t i = 1; i < count; ++i) {
        if (values[i] < *min_value) {
            *min_value = values[i];
        }
        if (values[i] > *max_value) {
            *max_value = values[i];
        }
    }
    return 1;
}

int main(void)
{
    int values[] = {4, 9, 1, 7};
    int minimum;
    int maximum;

    printf("empty=%d\n", min_max(NULL, 0, &minimum, &maximum));
    if (min_max(values, sizeof values / sizeof values[0],
                &minimum, &maximum)) {
        printf("min=%d max=%d\n", minimum, maximum);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o min_max
./min_max
```
## 6. 코드 해석
empty input은 `values[0]` 전에 실패한다. nonempty range에서는 첫 원소로 두 결과를 초기화하고 나머지를 검사한다. 출력은 `empty=0`, `min=1 max=9`다.
## 7. 내부 동작
**[Algorithm]** iteration 시작 시 output은 `[0, i)` prefix의 minimum·maximum이다. 전체 range 뒤 결과 조건을 만족한다. time은 `O(n)`, auxiliary space는 `O(1)`이며 `n`은 원소 수다.

**[C17]** read-only input에는 `const`를 사용한다. `sizeof values`를 function 안에서 element count로 사용하지 않는다. 함수는 NULL arguments를 거부하지만 non-NULL pointer의 lifetime·extent·writability나 overlap은 판별하지 못한다. 서로 겹치지 않는 output objects는 success 이후에만 결과를 제공한다.

**[MIPS — 수업 기준]** source comparison count는 MIPS instruction count와 같지 않다. 실제 calling convention과 registers는 MIPS target ABI 영역이다.

**[RISC-V — 병행 학습]** source comparison count는 RISC-V instruction count와 같지 않다. 실제 calling convention과 registers는 RISC-V target ABI 영역이다.
## 8. 자주 하는 실수
- `count == 0`인데 `values[0]`으로 초기화한다.
- function parameter에 `sizeof values`를 적용한다.
- non-NULL output pointers의 lifetime·writability·alias contract를 생략한다.
- min/max를 찾는 데 불필요한 dynamic allocation을 사용한다.
- warning을 없애려고 count나 index를 근거 없이 cast한다.
## 9. 필수 실습
empty, single, all equal, negative 포함, 일반 입력에서 status와 output을 검증한다.
[29-12 exercise](../../exercises/29-basic-algorithms/29-12/README.md)
## 10. 추가 실습
- ★ comparison 횟수를 센다.
- ★★ caller가 결과를 출력하는 별도 함수를 작성한다.
- ★★★ min·max를 struct result로 표현하는 설계와 비교한다.
## 11. 확인 문제
1. element count를 별도로 전달하는 이유는?
2. empty input에서 output은 언제 유효한가?
3. input parameter에 `const`를 붙이는 이유는?
4. invariant가 어떤 prefix를 요약하는가?
5. dynamic allocation이 필요하지 않은 이유는?
## 12. 핵심 정리
- pointer, count, status, outputs의 contract를 함께 설계한다.
- empty input은 first-element access 전에 처리한다.
- algorithm function을 I/O와 분리해 boundary tests를 쉽게 만든다.
## 13. 다음 Step
[29-13. Part 29 종합 복습](29-13-part-29-review.md)
## 14. 참고 자료
- N1570 6.7.6.3, 6.5.2.1. N1570은 **C11 공개 Committee Draft**이며 parameter adjustment·array access 규칙은 C17에서도 유지된다.
- [cppreference: array declaration](https://en.cppreference.com/w/c/language/array)
