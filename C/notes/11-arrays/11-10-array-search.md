# 11-10. 배열 검색

선형 검색은 index 0부터 순서대로 element를 비교해 찾을 값과 일치하는 첫 위치를 찾는다.

## 1. 학습 목표
- 찾을 값·현재 index·일치 여부를 추적한다.
- 찾으면 `break`로 순회를 끝낸다.
- 찾지 못한 상태를 유효 index와 겹치지 않게 표현한다.

## 2. 선수 지식
Part 8의 조건문, Part 9의 `break`, Step 11-5의 count를 사용한다.

## 3. 핵심 개념
각 `values[i]`를 target과 비교한다. 찾은 index를 저장하고 반복을 종료한다. `found_index = count`는 유효 index 0~count-1 밖의 sentinel 상태로 사용할 수 있다. 이 sentinel은 검색 상태용 정수값이며 pointer sentinel을 의미하지 않는다.

## 4. 문법
```c
size_t found_index = count;
for (size_t i = 0; i < count; ++i) {
    if (values[i] == target) {
        found_index = i;
        break;
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {4, 7, 1, 7, 9};
    size_t count = sizeof(values) / sizeof(values[0]);
    int target = 7;
    size_t found_index = count;

    for (size_t i = 0; i < count; ++i) {
        if (values[i] == target) {
            found_index = i;
            break;
        }
    }

    if (found_index < count) {
        printf("found at %zu\n", found_index);
    } else {
        printf("not found\n");
    }
    return 0;
}
```

## 6. 코드 해석
index 0의 4는 target과 다르고 index 1의 7은 일치한다. `found_index`를 1로 바꾸고 `break`하므로 뒤의 두 번째 7은 검사하지 않는다. 첫 일치 위치 1을 출력한다.

## 7. 내부 동작
[C17 표준] 모든 비교 전에 `i < count`가 참이므로 subscript가 유효하다. 찾지 못하면 `found_index == count`가 유지되며 이 값으로 배열을 접근하지 않는다. library search나 pointer는 이번 범위에 필요하지 않다.

## 8. 자주 하는 실수
- 찾지 못한 sentinel `count`를 subscript로 사용한다.
- `break` 없이 마지막 일치 위치를 저장하면서 첫 위치라고 설명한다.
- found 상태를 초기화하지 않는다.
- 배열 전체를 `==`로 target과 비교하려 한다.

## 9. 필수 실습
5개 배열에서 target 7의 첫 index를 찾고 없는 target도 확인한다. [실습 README](../../exercises/11-arrays/11-10/README.md)

## 10. 추가 실습
- ★ 첫 element 검색
- ★★ 없는 값 검색
- ★★★ 모든 일치 element 개수 세기

## 11. 확인 문제
1. 검색 시작 index는?
2. `found_index = count`를 쓰는 이유는?
3. sentinel을 array subscript로 사용해도 되는가?
4. 예제는 첫 일치와 마지막 일치 중 무엇을 찾는가?
5. target이 없으면 무엇을 출력하는가?

## 12. 핵심 정리
선형 검색은 유효 index를 순서대로 비교하고, first match 또는 안전한 not-found 상태를 명확히 남긴다.

## 13. 다음 Step
[Step 11-11. Part 11 종합 복습](11-11-part-11-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.9, 6.8.6.3
- [cppreference: Comparison operators](https://en.cppreference.com/w/c/language/operator_comparison.html)
