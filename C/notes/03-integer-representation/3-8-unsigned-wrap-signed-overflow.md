# Step 3-8. unsigned 순환과 signed overflow

C17에서 unsigned 산술의 modulo 규칙과 signed 정수 overflow의 Undefined Behavior를 구별한다.

## 1. 학습 목표

- unsigned 산술의 법을 최댓값보다 1 큰 수로 설명한다.
- `UINT_MAX + 1U`와 `0U - 1U`를 예측한다.
- signed overflow가 UB임을 설명한다.
- 산술과 정수형 변환을 구별한다.
- 구현별 숫자와 표준 관계를 분리한다.

## 2. 선수 지식

[3-7](3-7-same-bits-signed-unsigned.md)의 값 변환, `<limits.h>`, `UINT_MAX`, `INT_MAX`, `U` 접미사를 사용한다. 작은 unsigned 형의 승격은 Part 6에서 자세히 다룬다.

## 3. 핵심 개념

**[C17 표준]** unsigned 산술은 해당 형의 최댓값보다 1 큰 수를 법으로 한다.

```text
UINT_MAX + 1U == 0U
0U - 1U == UINT_MAX
```

법 `UINT_MAX+1`은 수학 설명이다. C 식 `UINT_MAX + 1U`의 결과는 이미 0이다.

**[Undefined Behavior]** `INT_MAX + 1`, `INT_MIN - 1`처럼 signed 연산의 수학적 결과가 결과형 범위를 벗어나면 C17은 결과를 정의하지 않는다. 2의 보수 CPU에서 특정 음수가 관찰되어도 C의 보장이 아니다.

정수 변환은 별도다. -1을 unsigned로 변환하면 `UINT_MAX`이고, 표현 불가능한 값을 signed로 변환하면 구현 정의 결과 또는 구현 정의 신호다.

## 4. 문법

```c
unsigned int maximum = UINT_MAX;
unsigned int wrapped = maximum + 1U;
unsigned int below_zero = 0U - 1U;
```

`unsigned char`나 `unsigned short`는 계산 전에 승격될 수 있으므로 이번에는 연산형이 분명한 `unsigned int`를 사용한다.

## 5. 최소 코드 예제

```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    unsigned int maximum = UINT_MAX;
    unsigned int wrapped = maximum + 1U;
    unsigned int below_zero = 0U - 1U;

    printf("maximum=%u\n", maximum);
    printf("wrapped=%u\n", wrapped);
    printf("below_zero=%u\n", below_zero);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_boundaries.c -o unsigned_boundaries && ./unsigned_boundaries
```

## 6. 코드 해석

`wrapped`는 0이고 `below_zero`는 `maximum`과 같다. 예제에는 signed 경계를 넘는 식이 없다. 정확한 `UINT_MAX` 숫자는 구현마다 다를 수 있다.

## 7. 내부 동작

- **[C 표준]** 식의 연산형을 정한 뒤 unsigned modulo 또는 signed 범위 규칙을 적용한다.
- **[컴파일러]** signed overflow가 없는 정의된 프로그램을 전제로 최적화할 수 있다.
- **[CPU]** 하위 bit 결과가 같아 보여도 C의 signed·unsigned 의미는 다르다.
- **[메모리]** 이 예제는 객체 표현이나 byte 순서를 검사하지 않는다.
- **[업무 의미]** 정의된 unsigned 순환도 재고·길이 계산에서는 논리 오류일 수 있다.

## 8. 자주 하는 실수

- 모든 정수 overflow가 순환한다고 한다.
- `UINT_MAX + 1U`를 법 자체라고 한다.
- `0U - 1U` 결과를 -1이라고 한다.
- `INT_MAX + 1`을 실행해 결과를 조사한다.
- 경고가 없으면 UB가 없다고 생각한다.
- unsigned 순환이 업무상 항상 안전하다고 한다.

## 9. 필수 실습

- **목적:** 정의된 unsigned 경계를 안전하게 관찰한다.
- **작성할 파일:** `exercises/03-integer-representation/3-8/unsigned_boundaries.c`.
- **해야 할 일:** [README](../../exercises/03-integer-representation/3-8/README.md)에 따라 세 값을 출력하고 signed 경계 식은 실행 없이 분류한다.
- **예상 관찰:** 최댓값 다음은 0, 0 아래는 최댓값이다.

## 10. 추가 실습

- ★ `UINT_MAX + 2U`를 종이에서 설명
- ★★ `ULONG_MAX`로 같은 관계 관찰
- ★★★ GCC `-fwrapv`와 ISO C17 규칙 비교

## 11. 확인 문제

1. unsigned int 산술의 법은?
2. `UINT_MAX + 1U` 결과는?
3. `0U - 1U` 결과는?
4. `INT_MAX + 1`의 분류는?
5. `unsigned int u = -1;`은 signed overflow인가?
6. 정의된 unsigned 순환은 업무상 항상 옳은가?

<details><summary>정답과 해설</summary>

1. `UINT_MAX+1`. 2. 0. 3. `UINT_MAX`. 4. UB. 5. 아니며 정의된 unsigned 변환이다. 6. 아니다.

</details>

## 12. 핵심 정리

unsigned 산술은 modulo로 정의되지만 signed overflow는 UB다. 변환 규칙은 산술 overflow와 별개이며, 정의된 결과와 프로그램 의미의 타당성도 구별한다.

## 13. 다음 Step

[Step 3-9. Part 3 종합 복습](3-9-part-3-review.md)

## 14. 참고 자료

- [WG14 N2176 C17 투표 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.3.1.1, 6.3.1.3, 6.5.
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): C11 공개 초안이며 위 C17 공통 규칙의 참고 자료다.
- [GCC Code Generation Options](https://gcc.gnu.org/onlinedocs/gcc/Code-Gen-Options.html#index-fwrapv)
