# 4-7. Part 4 종합 복습

Part 4는 정수형 이름을 외우는 과정이 아니라 필요한 계약을 선택하는 과정이다. 일반 계산, 정확한 폭, 최소 범위, 구현이 선택한 빠른 형, 포인터 변환 목적을 구별하고 범위·상수·출력 매크로까지 한 흐름으로 연결한다.

## 1. 학습 목표

- `<stdint.h>`의 네 정수형 계열을 목적에 따라 선택한다.
- exact-width typedef의 제공 조건과 C byte의 의미를 설명한다.
- signed·unsigned exact-width 형의 범위와 산술 주의를 말한다.
- 한계·상수·출력 매크로를 올바르게 조합한다.
- 레지스터 값 모델과 실제 하드웨어 접근을 구별한다.
- C17 표준 보장과 현재 구현의 관찰을 분리한다.

## 2. 선수 지식

Step 4-1~4-6 전체를 복습한다. Part 2의 기본 정수형·`sizeof`·`CHAR_BIT`·`<limits.h>`, Part 3의 정수 표현과 unsigned 모듈러 산술도 연결한다.

## 3. 핵심 개념

### 3.1 형 선택 지도

| 요구 | 먼저 검토할 형 | 핵심 질문 |
|---|---|---|
| 일반 계산 | `int` | 외부 폭 계약이 정말 필요한가? |
| 정확히 N bit | `intN_t`, `uintN_t` | 대상 구현이 이 형을 제공하는가? |
| 적어도 N bit, 작은 폭 | `int_leastN_t`, `uint_leastN_t` | 더 넓어도 괜찮은가? |
| 적어도 N bit, 빠른 선택 | `int_fastN_t`, `uint_fastN_t` | 구현 선택을 받아들일 수 있는가? |
| 변환된 포인터 값 보관 | `intptr_t`, `uintptr_t` | 대상 구현이 제공하며 실제로 변환이 필요한가? |

무조건 `int32_t`를 쓰는 규칙은 없다. 요구가 일반 계산뿐이라면 `int`가 더 자연스러울 수 있다.

### 3.2 exact-width 조건

`int8_t`, `int16_t`, `int32_t`, `int64_t`와 unsigned 대응형은 **조건을 만족하는 형이 있을 때만** 제공된다.

- 정확히 N bit
- padding bit 없음
- signed exact-width는 2의 보수 표현

`int8_t`가 존재한다는 사실은 그 구현에 정확한 8-bit 정수형이 있다는 뜻이다. 그렇다고 C17 전체에서 1 C byte를 8 bit로 고정하지는 않는다. `sizeof`의 단위는 C byte이고 bit 수는 `CHAR_BIT`로 연결한다.

### 3.3 least와 fast

`int_least16_t`의 16은 최소 요구 범위다. 실제 형은 16 bit보다 넓을 수 있다. `int_fast8_t`의 8도 최소 요구 범위이며 구현이 빠른 연산에 알맞다고 선택한 형은 더 넓을 수 있다.

따라서 다음 두 식은 이름만으로 참이라고 결론 낼 수 없다.

```text
sizeof(int_least16_t) * CHAR_BIT == 16
sizeof(int_fast8_t) * CHAR_BIT == 8
```

### 3.4 한계·상수·출력

| 목적 | 32-bit signed | 32-bit unsigned |
|---|---|---|
| 최솟값 | `INT32_MIN` | 0 |
| 최댓값 | `INT32_MAX` | `UINT32_MAX` |
| 상수 | `INT32_C(...)` | `UINT32_C(...)` |
| 10진 출력 | `PRId32` | `PRIu32` |
| 16진 출력 | 필요에 맞는 signed 변환 검토 | `PRIx32` |

`<stdint.h>`는 typedef·한계·상수 매크로를 제공한다. `<inttypes.h>`는 이를 포함하고 `PRI`·`SCN` 형식 매크로 등을 추가한다.

### 3.5 정확한 폭의 한계

정확한 폭은 파일, 통신, 레지스터 값의 폭을 모델링하는 데 유용하다. 그러나 다음을 자동으로 해결하지 않는다.

- 파일·통신의 byte 순서
- 실제 하드웨어 주소와 접근 부작용
- signed overflow
- 작은 정수형의 정수 승격
- 잘못된 업무상 범위

## 4. 문법

```c
#include <inttypes.h>
#include <stdio.h>

int32_t offset = -INT32_C(120);
uint32_t register_value = UINT32_C(0xA5A50001);

printf("offset=%" PRId32 "\n", offset);
printf("register=0x%08" PRIx32 "\n", register_value);
```

`<inttypes.h>` 하나로 필요한 `<stdint.h>` 정의도 사용할 수 있다. 상수·변수·출력 서식을 한 계약에 맞추는 것이 중요하다.

## 5. 최소 코드 예제

`part4_review.c`:

```c
#include <inttypes.h>
#include <limits.h>
#include <stdio.h>

int main(void)
{
    int ordinary_count = 12;
    int_least16_t minimum_range = INT16_C(30000);
    int_fast16_t fast_range = INT16_C(30000);
    int32_t signed_value = -INT32_C(123456);
    uint32_t register_value = UINT32_C(0xA5A50001);

    printf("ordinary=%d\n", ordinary_count);
    printf("least=%lld fast=%lld\n",
           (long long)minimum_range, (long long)fast_range);
    printf("signed=%" PRId32 "\n", signed_value);
    printf("register=0x%08" PRIx32 "\n", register_value);
    printf("uint32 max=%" PRIu32 "\n", UINT32_MAX);
    printf("uint32 bytes=%zu CHAR_BIT=%d\n",
           sizeof register_value, CHAR_BIT);
    return 0;
}
```

이 예제는 `int32_t`와 `uint32_t`가 제공되는 구현에서 번역된다. 실제 하드웨어 접근, 포인터, bit 연산은 포함하지 않는다.

## 6. 코드 해석

1. `ordinary_count`는 폭 계약이 없는 일반 계산용이므로 `int`다.
2. least와 fast 변수는 같은 최소 범위를 요구하지만 구현이 서로 다른 바탕형을 선택할 수 있다.
3. signed exact-width 값은 `INT32_C`와 `PRId32`를 사용한다.
4. 레지스터 값 모델은 `UINT32_C`와 `PRIx32`를 사용한다.
5. `UINT32_MAX`는 하드코딩 없이 unsigned 32-bit 최댓값을 제공한다.
6. `sizeof register_value`는 C byte 수이고 `CHAR_BIT`는 byte당 bit 수다.
7. 출력 결과는 값과 구현 선택을 관찰하지만 메모리 byte 순서를 보여 주지 않는다.

## 7. 내부 동작

**[전처리]** 헤더의 typedef 선언과 한계·상수·서식 매크로가 번역 단위에 들어오며 `PRI` 문자열 조각이 전체 형식 문자열로 결합된다.

**[C 추상 기계]** exact-width 형은 정확한 값 폭을 제공한다. least와 fast 형은 최소 범위 조건을 제공하며 실제 폭은 더 넓을 수 있다.

**[컴파일러·ABI]** typedef의 바탕 기본형과 fast 계열 선택은 대상에 따라 다를 수 있다. 출력 매크로는 이 구현 선택을 형식 문자열에 반영한다.

**[CPU·하드웨어]** 컴파일러는 변수를 실제 CPU 레지스터나 메모리에 배치할 수 있지만, `register_value`라는 이름은 장치 레지스터 접근을 만들지 않는다.

**[OS·입출력]** `printf`는 값을 문자로 표시한다. 출력의 16진 숫자 순서는 객체의 byte 순서나 외부 파일 형식을 증명하지 않는다.

## 8. 자주 하는 실수

- 일반 계산에도 언제나 exact-width 형이 더 좋다고 가르친다.
- 모든 C17 구현에 모든 exact-width typedef가 있다고 단정한다.
- `int_least16_t`와 `int_fast16_t`를 정확히 16 bit라고 말한다.
- `sizeof(uint32_t)`의 숫자를 bit 수로 읽는다.
- `int32_t`를 무조건 `%d`, `uint32_t`를 무조건 `%u`로 출력한다.
- `INT32_C`를 단순한 “타입 접미사 문법”이라고 설명한다.
- `uint32_t` 변수만으로 실제 장치 접근이 완성된다고 생각한다.
- exact-width 값이 외부 byte 순서까지 자동으로 정한다고 생각한다.

## 9. 필수 실습

### Part 4 형 선택과 출력 카드

- **목적:** 일반형, least, fast, exact-width를 한 프로그램에서 목적별로 구별하고 표준 매크로로 출력한다.
- **해야 할 일:** 최소 예제의 다섯 변수를 직접 작성하고 출력한다. 별도 표에 각 변수의 요구, 제공 조건, 범위, 출력 방법을 기록한다.
- **사용할 개념:** `int`, least-width, fast-width, exact-width, 한계·상수·`PRI` 매크로, C byte, `CHAR_BIT`.
- **예상 관찰 결과:** 값은 의도대로 출력되고 least와 fast의 저장 크기는 구현 선택으로 기록된다.
- **확인 포인트:** 표준 보장, 구현 관찰, 아직 다루지 않은 외부 계약을 서로 구별했는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-7/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** 다섯 사용 사례를 exact, least, fast, 일반 `int` 중 하나와 연결하고 이유를 적는다.
- ★★ **응용:** `int_least8_t`와 `int_fast8_t`의 크기를 비교하고 이름의 8이 뜻하는 바를 설명한다.
- ★★★ **도전:** 32-bit 통신 필드 설계에서 exact-width 형이 해결한 문제와 남은 문제를 각각 두 가지 적는다.

## 11. 확인 문제

1. `int32_t`는 모든 C17 구현에서 반드시 존재하는가?
2. `int_fast8_t`는 정확히 8 bit인가?
3. `int_least16_t`는 정확히 16 bit인가?
4. `sizeof(uint32_t)`의 단위는 무엇인가?
5. `<stdint.h>`와 `<inttypes.h>`의 역할은 어떻게 다른가?
6. `uint32_t register_value`만으로 실제 하드웨어 레지스터를 읽는가?
7. 일반 계산에서 `int`를 계속 사용할 수 있는 이유는 무엇인가?

<details>
<summary>정답과 해설</summary>

1. 아니다. 정확한 32-bit이며 padding 없는 대응형이 있을 때 제공된다.
2. 아니다. 8은 최소 요구 범위다.
3. 아니다. 적어도 16-bit 요구 범위를 만족하는 가장 작은 폭이다.
4. C byte다.
5. `<stdint.h>`는 폭 기반 typedef·한계·상수 매크로를 제공하고 `<inttypes.h>`는 이를 포함·확장해 형식화된 입출력 매크로 등을 제공한다.
6. 아니다. 평범한 값 객체일 뿐이다.
7. 일반 계산에는 정확한 외부 폭 계약이 필요하지 않을 수 있고 `int`가 구현의 자연스러운 기본 정수형이기 때문이다.

</details>

## 12. 핵심 정리

Part 4의 핵심은 이름보다 계약이다. exact는 정확한 폭이 있는 구현에서만 제공되고, least와 fast의 숫자는 최소 요구 범위다. `sizeof`는 C byte 단위이며 `CHAR_BIT`와 함께 해석한다. 상수·한계·출력에는 `<stdint.h>`와 `<inttypes.h>`의 매크로를 사용하고, 정확한 폭이 실제 장치 접근이나 byte 순서까지 해결한다고 과장하지 않는다.

## 13. 다음 Step

Step 5-1. `printf` 서식 문자열과 인자 대응

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.8과 7.20의 typedef·한계·상수·형식 매크로를 종합 확인한다.
- [ISO/IEC 9899:2018](https://www.iso.org/standard/74528.html): C17 공식 표준의 서지 정보다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): Part 4의 형과 매크로를 한 표에서 복습한다.
- [GCC: C implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html): GCC 대상의 구현 선택을 표준 보장과 구별해 확인한다.
