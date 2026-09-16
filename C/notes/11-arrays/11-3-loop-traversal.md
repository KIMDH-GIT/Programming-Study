# 11-3. 반복문으로 배열 순회

배열의 연속된 index 범위는 `for` 반복문의 초기화·조건·변화와 자연스럽게 대응한다.

## 1. 학습 목표
- index 0부터 마지막 element까지 순회한다.
- `i < element_count` 조건으로 off-by-one을 피한다.
- 각 반복의 index와 element 값을 추적한다.

## 2. 선수 지식
Part 9의 `for`와 off-by-one, Step 11-2의 indexing을 사용한다.

## 3. 핵심 개념
5개 배열을 순회하려면 `i = 0`, `i < 5`, `++i`를 사용한다. 각 반복에서 `values[i]`는 현재 element다. `i <= 5`는 index 5까지 접근하려 하므로 잘못이다. element count와 조건의 상한을 같은 의미로 유지해야 한다.

## 4. 문법
```c
for (int i = 0; i < element_count; ++i) {
    use(array[i]);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};

    for (int i = 0; i < 5; ++i) {
        printf("index %d: %d\n", i, values[i]);
    }
    return 0;
}
```

## 6. 코드 해석
`i`는 0, 1, 2, 3, 4가 되고 각 index의 값 10~50을 출력한다. `i == 5`가 되면 조건이 거짓이므로 `values[5]`는 평가하지 않는다.

| 반복 | `i` | 접근 element | 값 |
|---:|---:|---|---:|
| 1 | 0 | `values[0]` | 10 |
| 2 | 1 | `values[1]` | 20 |
| 3~5 | 2~4 | `values[2]`~`values[4]` | 30~50 |

## 7. 내부 동작
[C17 표준] 매 반복의 subscript가 유효한 배열 element를 지정해야 한다. 배열 element는 선언 순서대로 연속 배치되지만 CPU instruction이나 cache 동작은 C17이 정하지 않는다.

## 8. 자주 하는 실수
- `i = 1`에서 시작해 첫 element를 건너뛴다.
- `i <= 5`로 범위를 한 칸 넘는다.
- 반복 조건과 배열 count에 서로 다른 수를 쓴다.
- loop body에서 `i`를 추가로 변경한다.

## 9. 필수 실습
5개 정수 배열을 index와 함께 처음부터 끝까지 출력한다. [실습 README](../../exercises/11-arrays/11-3/README.md)

## 10. 추가 실습
- ★ 모든 element에 1 더해 출력
- ★★ 짝수 element만 출력
- ★★★ 순회 전 index·값 추적표 작성

## 11. 확인 문제
1. 순회 초기 index는?
2. 5개 배열의 반복 조건은 왜 `i < 5`인가?
3. `i == 5`일 때 body가 실행되는가?
4. `i <= 5`가 위험한 이유는?
5. 첫 element를 건너뛰는 초기값은?

## 12. 핵심 정리
배열 순회는 0부터 element count 미만까지이며, loop condition이 모든 subscript를 유효 범위에 둔다.

## 13. 다음 Step
[Step 11-4. 연속 메모리 배치와 원소 주소](11-4-contiguous-layout-addresses.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.8.5.3
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
