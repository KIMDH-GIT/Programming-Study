# 4-2. `int8_t`, `int16_t`, `int32_t`, `int64_t`

`intN_t` 계열은 제공되는 구현에서 정확히 N bit인 signed 정수형을 가리킨다. 이름의 숫자만 외우지 않고, 정확한 폭·padding 없음·선택 제공이라는 세 조건을 함께 읽는다.

## 1. 학습 목표

- `int8_t`, `int16_t`, `int32_t`, `int64_t`의 의미를 설명한다.
- exact-width signed typedef와 기본 signed 정수형의 관계를 설명한다.
- exact-width typedef가 선택적으로 제공된다는 사실을 코드와 문서에서 표시한다.
- `sizeof` 결과를 C byte 단위로 해석한다.
- signed exact-width 형의 표현 범위를 관련 한계 매크로와 연결한다.

## 2. 선수 지식

Step 4-1의 정수형 선택 기준과 Part 3의 signed 표현·범위 구분이 필요하다. C17은 일반 signed 정수형 전체를 2의 보수로 고정하지 않는다. 다만 exact-width typedef에는 padding bit가 없고 2의 보수 표현을 사용하는 형만 대응될 수 있다.

## 3. 핵심 개념

### 3.1 signed exact-width typedef

| typedef | 의미 | 대응 한계 매크로 |
|---|---|---|
| `int8_t` | 정확히 8 bit인 signed 정수형 | `INT8_MIN`, `INT8_MAX` |
| `int16_t` | 정확히 16 bit인 signed 정수형 | `INT16_MIN`, `INT16_MAX` |
| `int32_t` | 정확히 32 bit인 signed 정수형 | `INT32_MIN`, `INT32_MAX` |
| `int64_t` | 정확히 64 bit인 signed 정수형 | `INT64_MIN`, `INT64_MAX` |

각 이름은 새로운 내장형이 아니라 조건을 만족하는 기존 signed 정수형의 typedef다. 예를 들어 어떤 ABI에서는 `int32_t`가 `int`의 별칭일 수 있고 다른 ABI에서는 다른 기본 signed 정수형의 별칭일 수 있다. 프로그램은 별칭의 바탕형을 추측하지 말고 `int32_t`의 계약을 사용한다.

### 3.2 제공되는 경우의 보장

**[C17 기준]** exact-width typedef는 해당 폭을 정확히 표현하고 padding bit가 없는 정수형이 있을 때만 제공된다. signed exact-width 형은 2의 보수 표현을 사용한다. 따라서 `int32_t`가 있다면 값 범위는 -2147483648부터 2147483647까지다.

그러나 구현에 그런 형이 없으면 해당 typedef를 생략할 수 있다. “표준 헤더에 이름이 실려 있다”와 “모든 구현이 그 이름을 반드시 선언한다”는 다른 말이다.

### 3.3 `int8_t`와 C byte

`int8_t`가 있으려면 padding 없이 정확히 8 bit인 signed 정수형이 있어야 한다. C 정수형의 저장 크기는 C byte 단위이고 한 C byte는 `CHAR_BIT` bit다. 따라서 `CHAR_BIT`가 8보다 큰 구현에는 정확한 8-bit 정수형을 만들 수 없어 `int8_t`가 제공되지 않는다.

이 사실을 “C byte는 원래 8 bit다”라고 뒤집어 말하면 안 된다. 올바른 관계는 다음과 같다.

```text
C17 전체: CHAR_BIT >= 8
int8_t가 제공되는 구현: 정확한 8-bit signed 정수형이 존재
```

### 3.4 정확한 폭과 산술 안전성

정확한 폭은 범위를 명확히 하지만 signed overflow를 정의된 동작으로 바꾸지 않는다. `int32_t` 값에 대한 산술의 결과가 결과형 범위를 벗어나면 여전히 정의되지 않은 동작이 될 수 있다. 폭을 안다는 것과 범위 검사를 생략해도 된다는 것은 별개다.

## 4. 문법

```c
#include <stdint.h>

int8_t small = INT8_C(-12);
int16_t medium = INT16_C(-1200);
int32_t large = INT32_C(-120000);
int64_t very_large = INT64_C(-12000000000);
```

`INTN_C` 매크로는 Step 4-5에서 자세히 배운다. 여기서는 대응 폭의 상수를 작성할 때 임의의 접미사를 추측하지 않기 위한 표준 매크로로 사용한다.

## 5. 최소 코드 예제

`signed_widths.c`:

```c
#include <limits.h>
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    int8_t a = INT8_C(-8);
    int16_t b = INT16_C(-1600);
    int32_t c = INT32_C(-320000);
    int64_t d = INT64_C(-6400000000);

    printf("values=%lld,%lld,%lld,%lld\n",
           (long long)a, (long long)b, (long long)c, (long long)d);
    printf("bytes=%zu,%zu,%zu,%zu\n",
           sizeof a, sizeof b, sizeof c, sizeof d);
    printf("CHAR_BIT=%d\n", CHAR_BIT);
    return 0;
}
```

이 프로그램은 네 typedef가 모두 제공되는 구현을 대상으로 한다. `long long`은 C17에서 적어도 64 bit의 값 범위를 가지므로 예제 값들을 표현할 수 있으며, 변환 뒤 `%lld`와 형이 일치한다. exact-width 형의 직접 출력은 Step 4-5에서 배운다.

## 6. 코드 해석

1. 네 변수의 선언은 각각 필요한 정확한 폭을 소스에 드러낸다.
2. `INTN_C` 매크로는 대응 최소 폭 정수형에 적합한 상수 표현을 만든다.
3. 출력 직전에 각 값을 `long long`으로 변환해 `%lld`와 맞춘다.
4. `sizeof`는 각 객체의 C byte 수를 출력한다.
5. `sizeof a`가 1이면 1 C byte라는 뜻이다. `int8_t`가 존재하므로 이 구현의 그 byte는 정확히 8 bit다.
6. `sizeof d`의 숫자만 보고 다른 구현의 C byte 크기를 정하지 않는다.

## 7. 내부 동작

**[헤더·typedef]** `<stdint.h>`는 구현의 기존 정수형 중 조건을 만족하는 형에 `intN_t` 이름을 부여한다. typedef는 별도 런타임 객체나 변환 코드를 만들지 않는다.

**[표현]** exact-width signed 형은 padding 없이 정확한 폭을 가지며 2의 보수 표현을 사용한다. 이는 Part 3에서 일반 signed 정수형에 남아 있던 표현 선택지보다 강한 조건이다.

**[정수 승격]** `int8_t`나 `int16_t`가 `int`보다 좁으면 많은 식에서 `int`로 승격될 수 있다. 변수 선언의 폭과 중간 식의 형이 언제나 같다고 가정하지 않는다. 승격 규칙은 Part 6에서 자세히 배운다.

**[ABI]** 함수 호출에서 typedef의 바탕형이 무엇인지는 구현과 ABI에 달릴 수 있다. 그래서 직접 출력할 때 고정된 `%d`나 `%ld`를 추측하는 대신 Step 4-5의 `PRI` 매크로를 사용한다.

## 8. 자주 하는 실수

- 네 `intN_t`가 모든 C17 구현에 반드시 있다고 말한다.
- `int8_t`가 존재하지 않는 구현도 비표준이라고 판단한다.
- `sizeof(int32_t) == 4`를 C17 전체에 일반화한다.
- exact-width signed 형도 일반 signed 형처럼 부호와 크기 또는 1의 보수일 수 있다고 설명한다.
- `int32_t`를 썼으므로 signed overflow도 자동으로 순환한다고 생각한다.
- `int32_t`의 바탕형이 언제나 `int`라고 가정해 `%d`를 고정한다.

## 9. 필수 실습

### signed exact-width 형 관찰

- **목적:** 네 signed exact-width 이름의 폭 계약과 현재 구현의 C byte 크기를 구별한다.
- **해야 할 일:** 네 형의 변수를 음수 상수로 초기화하고 `long long`으로 변환해 출력한다. 각 객체의 `sizeof`와 `CHAR_BIT`를 기록한다.
- **사용할 개념:** `intN_t`, `INTN_C`, signed 범위, `sizeof`, `CHAR_BIT`.
- **예상 관찰 결과:** 흔한 8-bit-byte 구현에서 크기는 1, 2, 4, 8 C byte로 보인다. 정확한 폭은 typedef 계약이고 크기 숫자의 단위는 C byte다.
- **확인 포인트:** 선택 제공 조건, 2의 보수 조건, signed overflow 금지를 모두 기록했는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-2/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** `INT8_MIN`, `INT8_MAX`의 수학적 값을 표에 적고 8-bit 2의 보수 범위와 연결한다.
- ★★ **응용:** 네 형의 `sizeof(형) * CHAR_BIT`를 계산해 이름의 폭과 비교한다.
- ★★★ **도전:** 현재 구현에서 `int32_t`와 `int`의 `sizeof`가 같더라도 두 이름의 이식성 계약이 다른 이유를 설명한다.

## 11. 확인 문제

1. `int32_t`는 모든 C17 구현에서 반드시 존재하는가?
2. `int16_t`가 제공된다면 padding bit를 가질 수 있는가?
3. signed exact-width 형은 C17에서 어떤 음수 표현을 사용하는가?
4. `CHAR_BIT`가 16인 구현에서 `int8_t`를 제공할 수 있는가?
5. `int32_t`를 사용하면 범위를 넘는 signed 덧셈이 순환하도록 보장되는가?

<details>
<summary>정답과 해설</summary>

1. 아니다. 조건을 만족하는 형이 있을 때만 제공된다.
2. 없다. exact-width 형은 padding bit가 없어야 한다.
3. 2의 보수 표현이다.
4. 제공할 수 없다. 가장 작은 객체도 1 C byte, 즉 16 bit의 저장 공간을 차지하기 때문이다.
5. 아니다. 표현 범위를 벗어난 signed 산술은 여전히 정의되지 않은 동작이다.

</details>

## 12. 핵심 정리

`int8_t`, `int16_t`, `int32_t`, `int64_t`는 제공될 때 정확한 폭, padding 없음, signed 2의 보수 표현이라는 강한 계약을 준다. 그 대신 구현에 해당 형이 없으면 이름도 없을 수 있다. `sizeof`는 C byte 수이고, exact-width 형도 signed overflow 규칙을 바꾸지 않는다.

## 13. 다음 Step

[Step 4-3. `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`](4-3-unsigned-exact-width-integers.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.20.1.1 exact-width integer types와 7.20.2 한계를 확인한다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): signed exact-width typedef와 선택 제공 조건을 요약한다.
- [GCC: Integers implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html): GCC 대상의 기본 정수 표현 선택을 확인하는 구현 문서다.
