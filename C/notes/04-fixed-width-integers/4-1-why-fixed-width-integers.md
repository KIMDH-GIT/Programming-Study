# 4-1. 고정 폭 정수형이 필요한 이유

같은 `int`라도 구현에 따라 표현 범위와 저장 크기가 달라질 수 있다. 이번 Step에서는 일반 정수형이 나쁜 것이 아니라, 외부 형식처럼 **정확한 bit 폭이 계약의 일부인 상황**에서 `<stdint.h>`의 정수형을 검토해야 하는 이유를 배운다.

## 1. 학습 목표

- `int`의 폭이 모든 C17 구현에서 같지 않음을 설명한다.
- 일반 계산과 외부 형식 표현에 알맞은 정수형 선택 기준을 구별한다.
- `sizeof`의 결과가 C byte 단위임을 말한다.
- `<stdint.h>`가 제공하는 네 정수형 계열의 목적을 큰 틀에서 구별한다.
- exact-width 정수형이 모든 구현에 반드시 존재하지는 않음을 설명한다.

## 2. 선수 지식

Part 2의 기본 정수형, `sizeof`, `CHAR_BIT`, `<limits.h>`와 Part 3의 bit·byte·정수 범위를 사용한다. `sizeof(char) == 1`은 1 C byte를 뜻하고, 한 C byte의 bit 수는 `CHAR_BIT`이며 C17에서는 8 이상이다.

아직 포인터, 배열, bit 연산, `volatile`은 사용하지 않는다. 통신·파일·레지스터는 정확한 폭이 필요한 이유를 설명하는 짧은 사례로만 다룬다.

## 3. 핵심 개념

### 3.1 값의 의미와 표현 폭은 다른 요구다

나이, 반복 횟수, 메뉴 번호처럼 구현의 자연스러운 정수 계산에 잘 맞는 값에는 흔히 `int`가 적합하다. 반면 외부 문서가 “부호 없는 16-bit 필드”라고 정한 값은 단순히 0 이상인 정수가 아니라 **폭까지 포함한 계약**이다.

| 상황 | 먼저 검토할 형 | 이유 |
|---|---|---|
| 일반적인 정수 계산 | `int` | 구현에서 자연스럽고 효율적인 기본 정수형이다. |
| 정확히 N bit인 외부 필드 | `intN_t`, `uintN_t` | 그 typedef가 제공된다면 정확한 폭과 padding 없음이 보장된다. |
| 적어도 N bit 범위가 필요함 | `int_leastN_t`, `uint_leastN_t` | 요구 범위를 만족하는 가장 폭이 작은 형이다. |
| 적어도 N bit이면서 빠른 형 | `int_fastN_t`, `uint_fastN_t` | 구현이 속도를 고려해 선택한다. 더 넓을 수 있다. |
| 변환된 객체 포인터 값을 담는 정수 | `intptr_t`, `uintptr_t` | 제공되는 구현에서 해당 변환 용도를 지원한다. |

`N`은 최소 요구 폭 또는 정확한 폭을 나타내지만 계열마다 뜻이 다르다. 따라서 이름에 `32`가 있다는 이유만으로 모든 계열을 “정확히 32 bit”라고 읽으면 안 된다.

### 3.2 `<stdint.h>`가 해결하는 문제

`<limits.h>`는 `INT_MIN`, `INT_MAX`, `UINT_MAX`처럼 기본 정수형의 실제 범위를 알려 준다. `<stdint.h>`는 여기에 더해 폭 중심의 typedef와 관련 한계·상수 매크로를 제공한다. 구현이 지원한다면 소스에서 “정확히 32 bit”라는 의도를 `uint32_t`처럼 직접 표현할 수 있다.

하지만 `<stdint.h>`가 모든 시스템을 8-bit byte와 32-bit `int`로 바꾸는 것은 아니다. C 구현의 실제 자료형 중 조건을 만족하는 형에 새 typedef 이름을 붙인다.

### 3.3 exact-width가 유용한 곳

- 통신 프로토콜의 정해진 폭 필드
- 파일 형식의 정해진 폭 숫자
- 하드웨어 문서가 폭을 명시한 레지스터 모델

이 예들은 폭을 선택하는 이유만 보여 준다. byte 순서, padding이 있는 복합 자료, 실제 장치 접근은 이후 Part에서 별도로 다룬다. 정확한 폭의 정수 하나를 골랐다고 외부 byte 배열과의 호환성이 자동으로 완성되는 것은 아니다.

## 4. 문법

```c
#include <stdint.h>

int32_t signed_value = 0;
uint32_t unsigned_value = UINT32_C(0);
```

`int32_t`와 `uint32_t`는 현재 구현이 조건을 만족할 때 `<stdint.h>`가 선언하는 typedef 이름이다. `UINT32_C(0)`은 `uint_least32_t`로 승격된 형에 알맞은 정수 상수 표현을 만드는 매크로다. 자세한 한계·상수·출력 매크로는 Step 4-5에서 다룬다.

## 5. 최소 코드 예제

`fixed_width_reason.c`:

```c
#include <limits.h>
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    int ordinary_count = 12;
    uint32_t protocol_field = UINT32_C(12);

    printf("ordinary=%d\n", ordinary_count);
    printf("int bytes=%zu\n", sizeof ordinary_count);
    printf("uint32_t bytes=%zu\n", sizeof protocol_field);
    printf("CHAR_BIT=%d\n", CHAR_BIT);
    return 0;
}
```

이 예제는 `uint32_t`가 제공되는 구현에서 번역된다. 모든 C17 구현에서 이 typedef가 존재한다는 예제가 아니다.

## 6. 코드 해석

1. `<limits.h>`는 `CHAR_BIT`를 제공한다.
2. `<stdint.h>`는 구현이 지원하는 폭 기반 typedef와 관련 매크로를 선언한다.
3. `ordinary_count`는 일반 계산용 값이므로 `int`를 사용한다.
4. `protocol_field`는 정확한 32-bit 외부 필드를 모델링한다는 의도를 드러낸다.
5. 두 `sizeof` 결과는 모두 C byte 수다.
6. `sizeof protocol_field * CHAR_BIT`는 이 구현에서 객체가 차지하는 bit 수를 계산한다. `uint32_t`가 존재하면 padding 없이 정확히 32 bit이므로 그 곱은 32다.
7. 코드가 byte 순서나 실제 통신을 시험하지는 않는다.

## 7. 내부 동작

**[C17 기준]** `int`는 적어도 -32767부터 32767까지 표현하지만 정확한 폭과 저장 크기는 구현이 정한다. `<stdint.h>`의 exact-width typedef는 정확한 폭을 가지며 padding bit가 없는 정수형이 있을 때만 제공된다.

**[전처리·컴파일러]** 헤더를 포함하면 선언과 매크로가 번역 단위에 들어온다. typedef는 새 표현 방식을 만드는 것이 아니라 기존 정수형의 별칭이다.

**[ABI·CPU]** 어떤 대상에서는 32-bit 연산이 자연스럽고, 다른 대상에서는 더 넓은 형이 더 빠를 수 있다. 이 차이가 least-width와 fast-width 계열을 따로 둔 이유다.

**[외부 형식]** 정확한 폭은 계약의 한 부분일 뿐이다. byte 순서와 직렬화 규칙까지 확인해야 파일·통신 형식과 안전하게 연결된다.

## 8. 자주 하는 실수

- `int`는 언제나 32 bit라고 가정한다.
- `int` 대신 언제나 `int32_t`를 쓰는 것이 더 이식성 있다고 생각한다.
- 모든 C17 구현에 `int32_t`가 반드시 있다고 단정한다.
- `sizeof(uint32_t) == 4`를 표준의 직접 보장이라고 말한다. 결과 단위는 C byte이며 `CHAR_BIT`가 8이 아닐 수 있다.
- 정확한 폭만 맞으면 파일·통신 형식의 byte 순서도 자동으로 맞는다고 생각한다.
- `int_fast8_t`와 `int_least8_t`도 정확히 8 bit라고 읽는다.

## 9. 필수 실습

### 일반 계산과 외부 필드 구별하기

- **목적:** 값의 용도에 따라 `int`와 exact-width 형을 구별하고 C byte와 bit를 분리한다.
- **해야 할 일:** `type_choice.c`에 일반 개수용 `int`와 정확한 32-bit 외부 필드용 `uint32_t`를 선언한다. 두 객체의 `sizeof`와 `CHAR_BIT`를 출력하고, 각 결과의 단위를 기록한다.
- **사용할 개념:** `<stdint.h>`, `uint32_t`, `UINT32_C`, `sizeof`, C byte, `CHAR_BIT`.
- **예상 관찰 결과:** 두 값은 같은 숫자를 담을 수 있지만 선언은 서로 다른 요구를 표현한다. `sizeof`의 숫자는 C byte 수다.
- **확인 포인트:** “`int`는 항상 32 bit” 또는 “1 C byte는 항상 8 bit”라고 일반화하지 않았는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-1/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** `sizeof(short)`, `sizeof(int)`, `sizeof(long)`을 출력하고 모두 C byte 단위라고 표시한다.
- ★★ **응용:** 일반 계산, 16-bit 통신 필드, 최소 16-bit 범위, 빠른 최소 16-bit 계산에 각각 어떤 계열을 먼저 검토할지 표로 작성한다.
- ★★★ **도전:** 현재 구현에서 `sizeof(uint32_t) * CHAR_BIT`를 계산하고, 그 결과를 모든 C 구현의 byte 크기로 일반화할 수 없는 이유를 설명한다.

## 11. 확인 문제

1. 일반적인 반복 횟수에 `int`가 적합할 수 있는 이유는 무엇인가?
2. `int32_t`는 모든 C17 구현에서 반드시 존재하는가?
3. exact-width typedef가 제공되기 위한 핵심 조건 두 가지는 무엇인가?
4. `sizeof(uint32_t)`의 단위는 무엇인가?
5. `sizeof(char) == 1`은 한 byte가 반드시 8 bit라는 뜻인가?

<details>
<summary>정답과 해설</summary>

1. `int`는 구현의 자연스러운 기본 정수형이며 일반 계산에는 정확한 외부 폭 계약이 필요하지 않을 수 있다.
2. 아니다. 정확히 32 bit이고 padding이 없는 대응 정수형이 구현에 있을 때 제공된다.
3. 요구한 정확한 폭을 표현하고 padding bit가 없어야 한다.
4. C byte다.
5. 아니다. 한 C byte의 bit 수는 `CHAR_BIT`이며 C17은 8 이상만 보장한다.

</details>

## 12. 핵심 정리

고정 폭 정수형은 기본 정수형을 모두 대체하기 위한 도구가 아니다. 정확한 폭이 외부 계약의 일부일 때 exact-width 형을 검토하고, 최소 범위나 속도가 목적이면 least-width 또는 fast-width 계열을 검토한다. `sizeof`는 언제나 C byte를 세며 bit 수는 `CHAR_BIT`와 연결해 해석한다.

## 13. 다음 Step

[Step 4-2. `int8_t`, `int16_t`, `int32_t`, `int64_t`](4-2-signed-exact-width-integers.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.20.1의 정수형 정의와 5.2.4.2.1의 정수 한계를 확인한다.
- [ISO/IEC 9899:2018](https://www.iso.org/standard/74528.html): C17 공식 표준의 서지 정보다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): `<stdint.h>` 형 계열과 선택 제공 조건을 요약한다.
