# 4-4. 정확한 폭의 타입이 제공되는 조건

`<stdint.h>`에는 목적이 다른 여러 typedef 계열이 있다. exact-width 형이 없는 구현에서도 “적어도 N bit”라는 요구는 least-width 또는 fast-width 형으로 표현할 수 있다. 이름의 숫자가 의미하는 조건을 계열별로 구별하는 것이 이번 Step의 핵심이다.

## 1. 학습 목표

- exact-width typedef의 제공 조건을 정확히 설명한다.
- minimum-width와 fastest minimum-width 계열을 구별한다.
- `int_least16_t`와 `int_fast8_t`가 정확한 폭을 뜻하지 않음을 설명한다.
- `intptr_t`, `uintptr_t`의 제한된 목적과 선택 제공을 말한다.
- 필요한 계약에 따라 가장 알맞은 계열을 선택한다.

## 2. 선수 지식

Step 4-1~4-3의 exact-width 형, C byte, `CHAR_BIT`, signed·unsigned 범위를 사용한다. 포인터 자체와 포인터 변환 규칙은 아직 배우지 않았으므로 `intptr_t`와 `uintptr_t`는 목적만 소개한다.

## 3. 핵심 개념

### 3.1 네 계열의 계약

| 계열 | 예 | 숫자 16의 뜻 | 정확히 16 bit인가? |
|---|---|---|---|
| exact-width | `int16_t` | 정확한 폭 16 bit | 그렇다. 단, 형이 있을 때만 제공 |
| minimum-width | `int_least16_t` | 적어도 16 bit의 범위 | 아닐 수 있다. |
| fastest minimum-width | `int_fast16_t` | 적어도 16 bit의 범위를 만족하는 빠른 형 | 아닐 수 있다. |
| pointer-conversion capable | `intptr_t` | 폭 숫자를 이름에 표시하지 않음 | 포인터 변환 목적이며 선택 제공 |

signed 계열에는 대응하는 unsigned 계열 `uint16_t`, `uint_least16_t`, `uint_fast16_t`, `uintptr_t`가 있다.

### 3.2 exact-width 제공 조건

**[C17 기준]** `intN_t`와 `uintN_t`는 정확히 N bit이고 padding bit가 없는 정수형이 구현에 있을 때 제공된다. signed exact-width 형은 2의 보수 표현이다. 조건을 만족하는 형이 없으면 구현은 해당 이름을 제공하지 않는다.

따라서 다음 문장은 틀리다.

```text
모든 C17 구현에는 int8_t, int16_t, int32_t, int64_t가 있다.
```

반면 C17이 요구하는 최소 범위 때문에 8, 16, 32, 64에 대한 least-width와 fast-width typedef들은 제공된다. 이들은 정확한 폭 대신 최소 범위를 계약으로 삼는다.

### 3.3 minimum-width types

`int_leastN_t`는 적어도 N bit의 폭을 가진 signed 정수형 중 가장 작은 폭의 형이다. `uint_leastN_t`는 unsigned 대응형이다.

```text
int_least8_t    uint_least8_t
int_least16_t   uint_least16_t
int_least32_t   uint_least32_t
int_least64_t   uint_least64_t
```

예를 들어 구현에 정확한 16-bit 형은 없고 24-bit와 32-bit 형만 있다면 `int16_t`는 없을 수 있지만 `int_least16_t`는 24-bit 형일 수 있다. 따라서 `int_least16_t`를 “정확히 16 bit”라고 설명하면 안 된다.

### 3.4 fastest minimum-width types

`int_fastN_t`와 `uint_fastN_t`는 적어도 N bit의 요구를 만족하는 정수형 중 구현이 빠른 연산에 적합하다고 고른 형이다. “fast”는 모든 프로그램·CPU·상황에서 측정상 가장 빠르다는 보편 명제가 아니라 구현이 제공하는 typedef 선택이다.

```text
int_fast8_t    uint_fast8_t
int_fast16_t   uint_fast16_t
int_fast32_t   uint_fast32_t
int_fast64_t   uint_fast64_t
```

64-bit CPU에서 `int_fast8_t`가 64 bit이거나 32 bit일 수도 있다. 이름의 8은 **최소 요구 범위**이며 실제 폭을 고정하지 않는다.

### 3.5 `intptr_t`, `uintptr_t`

이 두 선택적 typedef는 `void *`로 변환할 수 있고 다시 되돌렸을 때 원래와 비교해 같은 포인터가 되는 signed·unsigned 정수형이다. 모든 객체 포인터의 숫자 주소를 일반 산술에 사용하라는 뜻이 아니다.

모든 C17 구현에 반드시 제공되는 형도 아니다. 포인터와 정수 사이의 변환 규칙은 이후 Pointer Part에서 자세히 학습한다.

## 4. 문법

```c
#include <stdint.h>

int_least16_t minimum_range = INT16_C(30000);
int_fast16_t fast_range = INT16_C(30000);
uint_least32_t unsigned_minimum = UINT32_C(4000000000);
uint_fast32_t unsigned_fast = UINT32_C(4000000000);
```

초기화 상수 매크로의 결과형은 exact-width typedef 자체가 아니라 대응하는 `int_leastN_t` 또는 `uint_leastN_t`의 promoted type에 맞는다. 자세한 규칙은 Step 4-5에서 다룬다.

## 5. 최소 코드 예제

`width_families.c`:

```c
#include <limits.h>
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    int_least16_t least_value = INT16_C(30000);
    int_fast16_t fast_value = INT16_C(30000);

    printf("values=%lld,%lld\n",
           (long long)least_value, (long long)fast_value);
    printf("bytes=%zu,%zu\n", sizeof least_value, sizeof fast_value);
    printf("bits=%zu,%zu\n",
           sizeof least_value * (size_t)CHAR_BIT,
           sizeof fast_value * (size_t)CHAR_BIT);
    return 0;
}
```

이 예제는 exact-width 형의 존재를 전제로 하지 않고 C17의 필수 least/fast typedef를 사용한다. 출력한 bit 수는 저장 공간 크기이며 값 비트 수와 무조건 같다고 단정하지 않는다.

## 6. 코드 해석

1. 두 변수 모두 적어도 16-bit 요구 범위를 만족한다.
2. `least_value`는 폭이 가장 작은 형, `fast_value`는 빠른 연산에 알맞다고 구현이 선택한 형이다.
3. 둘의 `sizeof`가 같을 수도 다를 수도 있다.
4. `sizeof * CHAR_BIT`는 저장 공간의 bit 수를 계산한다.
5. 이름의 `16`만으로 출력이 반드시 2 C byte 또는 16 bit라고 예측하지 않는다.
6. exact-width typedef나 포인터 변환을 사용하지 않으므로 그 선택 제공 여부와 무관하게 핵심 비교를 할 수 있다.

## 7. 내부 동작

**[C 표준]** 구현은 최소 요구 범위를 만족하는 기본 또는 확장 정수형을 찾아 least와 fast typedef를 정의한다. exact-width 형은 더 강한 조건을 만족할 때만 정의한다.

**[컴파일러·ABI]** `int_fast16_t`는 대상 CPU와 ABI가 효율적으로 다루는 형의 별칭일 수 있다. typedef 이름만으로 특정 기계 명령이나 성능 우위를 보장하지 않는다.

**[저장 표현]** least/fast 형은 exact-width와 달리 더 넓을 수 있고 padding 가능성도 exact-width 계약으로 제거되지 않는다. 실제 범위는 대응 `INT_LEASTN_MIN`, `INT_LEASTN_MAX`, `INT_FASTN_MIN`, `INT_FASTN_MAX` 매크로로 확인한다.

**[포인터 변환]** `intptr_t`와 `uintptr_t`는 구현이 적합한 정수형을 제공할 수 있을 때만 정의된다. 포인터의 유효성, 역참조, 수명은 이 typedef가 해결하지 않는다.

## 8. 자주 하는 실수

- `int_least16_t`를 정확히 16-bit 형이라고 말한다.
- `int_fast8_t`를 정확히 8-bit 형이라고 말한다.
- fast 계열이 언제나 least 계열보다 작거나 언제나 실제 측정에서 빠르다고 단정한다.
- exact-width 형이 없으면 `<stdint.h>` 자체를 쓸 수 없다고 생각한다.
- `intptr_t`, `uintptr_t`가 모든 구현에 있다고 단정한다.
- 포인터를 정수로 바꾸면 언제나 산술 가능한 물리 주소가 된다고 생각한다.

## 9. 필수 실습

### least와 fast의 실제 선택 비교

- **목적:** 최소 범위 계약과 구현이 선택한 저장 크기를 구별한다.
- **해야 할 일:** `int_least16_t`와 `int_fast16_t` 변수를 같은 값으로 초기화하고 값, C byte 크기, 저장 bit 수를 출력한다.
- **사용할 개념:** least-width, fast-width, `sizeof`, `CHAR_BIT`, typedef.
- **예상 관찰 결과:** 값은 같지만 두 형의 크기는 구현에 따라 같거나 다를 수 있다.
- **확인 포인트:** 어느 결과도 정확한 16 bit라고 미리 단정하지 않았는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-4/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** exact, least, fast의 숫자 N이 각각 무엇을 뜻하는지 한 문장씩 적는다.
- ★★ **응용:** `int_least8_t`, `int_fast8_t`, `int_least32_t`, `int_fast32_t`의 `sizeof`를 비교한다.
- ★★★ **도전:** exact-width 형이 없는 가상 24-bit-byte 구현에서 최소 16-bit 요구를 어느 계열로 표현할지 설명한다.

## 11. 확인 문제

1. `int_least16_t`는 반드시 정확히 16 bit인가?
2. `int_fast8_t`는 정확히 8 bit인가?
3. exact-width typedef가 생략될 수 있는 이유는 무엇인가?
4. least-width와 fast-width가 각각 우선하는 기준은 무엇인가?
5. `intptr_t`와 `uintptr_t`는 모든 C17 구현에서 제공되는가?
6. 이 두 포인터 관련 형을 이번 Part에서 깊게 다루지 않는 이유는 무엇인가?

<details>
<summary>정답과 해설</summary>

1. 아니다. 적어도 16 bit의 요구 범위를 만족하는 가장 작은 폭이다.
2. 아니다. 8은 최소 요구 범위이며 실제 형은 더 넓을 수 있다.
3. 정확한 폭이고 padding이 없는 대응 정수형이 구현에 없을 수 있기 때문이다.
4. least는 폭의 최소화, fast는 최소 범위를 만족하면서 구현이 빠르다고 선택한 형을 우선한다.
5. 아니다. 둘 다 선택적으로 제공된다.
6. 포인터 선언·유효성·변환 규칙을 아직 배우지 않았기 때문이다.

</details>

## 12. 핵심 정리

exact-width는 정확한 폭, least-width는 최소 요구 범위를 만족하는 가장 작은 폭, fast-width는 최소 요구 범위를 만족하는 빠른 형을 뜻한다. 이름의 숫자를 모든 계열에서 정확한 폭으로 읽지 않는다. `intptr_t`와 `uintptr_t`는 포인터 변환을 위한 선택적 정수형이며 Pointer Part에서 다시 다룬다.

## 13. 다음 Step

[Step 4-5. 범위·상수 매크로와 `<inttypes.h>` 출력](4-5-limits-constants-inttypes-output.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.20.1.1~7.20.1.4의 네 typedef 계열을 확인한다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): exact, least, fast, pointer-capable 형을 비교한다.
- [ISO/IEC 9899:2018](https://www.iso.org/standard/74528.html): C17 공식 표준의 서지 정보다.
