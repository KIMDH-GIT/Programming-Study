# 11-9. 역순 출력

역순 출력은 배열을 바꾸지 않고 마지막 유효 element부터 첫 element까지 index를 감소시키며 읽는다.

## 1. 학습 목표
- 마지막 유효 index를 count-1로 계산한다.
- unsigned `size_t`가 0 아래로 감소하지 않도록 반복을 작성한다.
- 역순 출력과 배열 element 재배치를 구분한다.

## 2. 선수 지식
Step 11-3의 순회, Step 11-5의 count, Part 4의 unsigned 동작을 안다.

## 3. 핵심 개념
count가 5이면 마지막 index는 4다. `size_t`는 unsigned이므로 `i >= 0` 조건은 종료 조건이 되지 않는다. 안전한 형태는 `i = count`에서 시작해 `i > 0`인 동안 `values[i - 1]`을 읽는 것이다. 출력 순서만 역방향이며 배열 저장 순서는 바뀌지 않는다.

## 4. 문법
```c
for (size_t i = count; i > 0; --i) {
    printf("%d\n", values[i - 1]);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    size_t count = sizeof(values) / sizeof(values[0]);

    for (size_t i = count; i > 0; --i) {
        printf("%d\n", values[i - 1]);
    }
    return 0;
}
```

## 6. 코드 해석
`i`는 5, 4, 3, 2, 1이고 실제 subscript는 `i - 1`이므로 4, 3, 2, 1, 0이다. 50부터 10까지 출력한다. `i == 0`에서는 body에 들어가지 않아 unsigned underflow 전에 끝난다.

## 7. 내부 동작
[C17 표준] `size_t`는 unsigned이므로 0에서 감소하면 modulo arithmetic으로 큰 값이 된다. 현재 반복은 0에서 `--i`를 실행하지 않는다. 배열을 읽기만 하므로 element 값과 배치는 그대로다.

## 8. 자주 하는 실수
- 첫 index를 count로 사용해 범위를 넘는다.
- `size_t i >= 0`을 종료 조건으로 사용한다.
- 역순 출력이 배열 자체를 뒤집는다고 생각한다.
- `i - 1`이 언제 평가되는지 추적하지 않는다.

## 9. 필수 실습
5개 배열을 마지막 element부터 첫 element까지 출력한다. [실습 README](../../exercises/11-arrays/11-9/README.md)

## 10. 추가 실습
- ★ 세 element 역순 출력
- ★★ 역순으로 index와 값 함께 출력
- ★★★ 위험한 `i >= 0` 코드를 실행 없이 분석

## 11. 확인 문제
1. count 5의 마지막 유효 index는?
2. 반복 첫 body에서 사용하는 subscript는?
3. `size_t i >= 0`이 종료되지 않는 이유는?
4. 이 예제가 element 순서를 실제로 바꾸는가?
5. `i == 0`일 때 `i - 1`을 평가하는가?

## 12. 핵심 정리
unsigned index의 역순 순회는 count에서 시작해 0보다 큰 동안 `i-1`을 사용하면 안전하다.

## 13. 다음 Step
[Step 11-10. 배열 검색](11-10-array-search.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.6
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
