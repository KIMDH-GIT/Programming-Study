# 4-5. 범위·상수 매크로와 `<inttypes.h>` 출력

폭 기반 정수형을 이식성 있게 쓰려면 typedef만이 아니라 범위, 소스 상수, 형식화된 출력도 그 형에 맞춰야 한다. `<stdint.h>`와 `<inttypes.h>`의 역할을 나누고 표준 매크로를 조합하는 방법을 배운다.

## 1. 학습 목표

- exact·least·fast 정수형의 한계 매크로를 읽는다.
- `INTN_C`와 `UINTN_C`의 역할을 정확히 설명한다.
- `PRId32`, `PRIu32`, `PRIx32`를 `printf` 형식 문자열에 조합한다.
- `<stdint.h>`와 `<inttypes.h>`의 관계를 구별한다.
- 기본형을 추측한 고정 서식이 왜 이식성을 해칠 수 있는지 설명한다.

## 2. 선수 지식

Step 4-1~4-4의 typedef 계열과 Part 2의 `<limits.h>` 매크로, Part 1의 문자열 리터럴 인접 결합과 `printf`를 사용한다. 정교한 형식 문자열 전체는 Part 5에서 다시 배운다.

## 3. 핵심 개념

### 3.1 정수 한계 매크로

`<limits.h>`의 `INT_MIN`, `INT_MAX`, `UINT_MAX`는 기본 `int`, `unsigned int`의 범위를 나타낸다. `<stdint.h>`의 다음 매크로는 폭 기반 typedef의 범위를 나타낸다.

```text
INT8_MIN   INT8_MAX   UINT8_MAX
INT16_MIN  INT16_MAX  UINT16_MAX
INT32_MIN  INT32_MAX  UINT32_MAX
INT64_MIN  INT64_MAX  UINT64_MAX
```

exact-width typedef가 제공되지 않으면 그 형에 대응하는 한계 매크로도 제공되지 않는다. least와 fast 계열에는 다음 형태가 있다.

```text
INT_LEAST16_MIN  INT_LEAST16_MAX  UINT_LEAST16_MAX
INT_FAST16_MIN   INT_FAST16_MAX   UINT_FAST16_MAX
```

구체적 숫자를 하드코딩하는 대신 매크로를 사용하면 현재 구현이 선택한 형의 계약을 그대로 읽을 수 있다.

### 3.2 정수 상수 매크로

```c
INT8_C(12)       UINT8_C(12)
INT16_C(1200)    UINT16_C(1200)
INT32_C(120000)  UINT32_C(120000)
INT64_C(12000000000) UINT64_C(12000000000)
```

이들은 C의 새 문법이나 “변수 뒤에 타입을 붙이는 기능”이 아니다. `<stdint.h>`가 정의하는 함수형 매크로이며, 인수인 **접미사 없는 정수 상수**에서 대응하는 `int_leastN_t` 또는 `uint_leastN_t`의 promoted type을 가진 정수 상수 표현을 만든다.

음수는 보통 다음처럼 양의 상수 매크로에 단항 `-`를 적용한다.

```c
int32_t temperature = -INT32_C(25);
```

매크로가 반드시 눈에 보이는 `L`, `LL`, `U` 문자열 하나를 덧붙인다고 일반화하지 않는다. 필요한 전개는 구현과 대응 형에 달릴 수 있다.

### 3.3 `<inttypes.h>`의 출력 매크로

`printf`는 가변 인수의 실제 형과 서식 지정자가 맞아야 한다. `int32_t`의 바탕형이 항상 `int`라고 보장되지 않으므로 `%d`를 무조건 고정하면 이식성이 깨질 수 있다.

`<inttypes.h>`는 `<stdint.h>`의 기능을 포함하고 폭 기반 정수형의 형식화된 입출력을 위한 매크로를 추가한다.

| 목적 | 매크로 | 조합 결과 예 |
|---|---|---|
| signed 10진 출력 | `PRId32` | `"%" PRId32` |
| unsigned 10진 출력 | `PRIu32` | `"%" PRIu32` |
| unsigned 16진 출력 | `PRIx32` | `"%" PRIx32` |

`PRId32` 자체에 `%`가 들어 있다고 생각하면 안 된다. `%`와 매크로를 인접 문자열 리터럴로 조합한다.

### 3.4 두 헤더의 역할

```text
<stdint.h>
정수 typedef, 관련 한계 매크로, 정수 상수 매크로

<inttypes.h>
<stdint.h>의 정의를 포함하고 PRI/SCN 형식 매크로와 intmax 관련 기능을 추가
```

이번 Step은 출력용 `PRI` 매크로만 사용한다. 입력용 `SCN` 매크로와 `scanf`는 Part 5에서 다룬다.

## 4. 문법

```c
#include <inttypes.h>
#include <stdio.h>

int32_t signed_value = -INT32_C(123);
uint32_t unsigned_value = UINT32_C(4000000000);

printf("%" PRId32 "\n", signed_value);
printf("%" PRIu32 "\n", unsigned_value);
printf("%" PRIx32 "\n", unsigned_value);
```

전처리 뒤에는 인접한 문자열 리터럴 조각이 하나의 형식 문자열이 된다. 구현이 `PRId32`를 `"d"`로 정의했다면 `"%" "d" "\n"`이 `"%d\n"`으로 합쳐지는 식이다. 실제 전개 문자열을 프로그램이 추측할 필요는 없다.

## 5. 최소 코드 예제

`portable_output.c`:

```c
#include <inttypes.h>
#include <stdio.h>

int main(void)
{
    int32_t signed_value = -INT32_C(123456);
    uint32_t unsigned_value = UINT32_C(4000000000);

    printf("signed=%" PRId32 "\n", signed_value);
    printf("unsigned=%" PRIu32 "\n", unsigned_value);
    printf("hex=%" PRIx32 "\n", unsigned_value);
    printf("range=%" PRId32 "..%" PRId32 "\n",
           INT32_MIN, INT32_MAX);
    return 0;
}
```

이 예제는 `int32_t`와 `uint32_t`가 제공되는 구현을 대상으로 한다.

## 6. 코드 해석

1. `<inttypes.h>`를 포함하면 `<stdint.h>`의 typedef·한계·상수 매크로도 사용할 수 있다.
2. `-INT32_C(123456)`은 양의 표준 상수 표현에 단항 마이너스를 적용한다.
3. `UINT32_C(4000000000)`은 대응 최소 폭 unsigned 형에 알맞은 정수 상수 표현이다.
4. `"signed=%" PRId32 "\n"`은 번역 과정에서 하나의 문자열 리터럴로 결합된다.
5. `PRIu32`는 unsigned 10진, `PRIx32`는 소문자 16진 출력을 요청한다.
6. `INT32_MIN`과 `INT32_MAX`는 하드코딩하지 않은 현재 형의 경계다.

## 7. 내부 동작

**[전처리]** 함수형 상수 매크로와 객체형 한계·서식 매크로가 소스 토큰으로 확장된다. `PRI` 조각은 인접 문자열 리터럴 결합 단계에서 전체 형식 문자열이 된다.

**[가변 인수]** `printf`는 인수의 형 정보를 별도로 전달받지 않는다. 형식 문자열과 실제 인수가 맞아야 하며, 불일치는 정의되지 않은 동작을 일으킬 수 있다.

**[ABI]** 같은 32-bit exact-width 형이라도 바탕형이 `int`, `long` 등으로 달라질 수 있다. `PRI` 매크로는 구현이 알맞은 길이 수정자와 변환 문자를 선택하게 한다.

**[표준과 구현]** 표준은 매크로의 역할과 결과형 조건을 정한다. 매크로가 실제로 어떤 문자 조각으로 전개되는지는 구현이 선택할 수 있다.

## 8. 자주 하는 실수

- `int32_t`를 항상 `%d`로 출력한다.
- `uint32_t`를 항상 `%lu`로 출력한다.
- `printf(PRId32, value)`처럼 `%` 없이 사용한다.
- `printf("%" PRId32, value)`의 인접 문자열 결합을 문자열 덧셈이라고 설명한다.
- `INT32_C`가 `int32_t` 형 자체를 반드시 만든다고 말한다. 기준은 `int_least32_t`의 promoted type이다.
- `<inttypes.h>`와 `<stdint.h>`를 서로 무관한 헤더라고 생각한다.
- exact-width 형이 없는데 대응 exact 한계·서식 매크로는 반드시 있다고 가정한다.

## 9. 필수 실습

### 한계·상수·출력 매크로 조합

- **목적:** 32-bit signed·unsigned 값을 표준 상수 매크로로 만들고 `PRI` 매크로로 출력한다.
- **해야 할 일:** signed 음수와 큰 unsigned 값을 만들고 10진·16진으로 출력한다. `INT32_MIN`, `INT32_MAX`, `UINT32_MAX`도 대응 서식으로 출력한다.
- **사용할 개념:** `INT32_C`, `UINT32_C`, 한계 매크로, `PRId32`, `PRIu32`, `PRIx32`, 문자열 리터럴 결합.
- **예상 관찰 결과:** 값과 범위가 잘리지 않고 대응 진법으로 출력된다.
- **확인 포인트:** `%`를 직접 쓰고 `PRI` 매크로를 바로 뒤에 인접시켰는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-5/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** 8·16·64 폭의 signed 최솟값, 최댓값, unsigned 최댓값 매크로 이름을 표로 적는다.
- ★★ **응용:** 같은 `uint32_t` 값을 `PRIu32`와 `PRIx32`로 출력해 값과 표기 진법을 구별한다.
- ★★★ **도전:** 현재 구현에서 전처리 결과를 관찰하고 `PRId32` 전개가 C17 전체의 고정 문자열이 아닌 이유를 설명한다.

## 11. 확인 문제

1. `INT_MIN`과 `INT32_MIN`은 각각 어느 형의 경계인가?
2. `INT32_C(10)`은 단순히 “변수에 타입을 붙이는 문법”인가?
3. `INT32_C` 결과는 반드시 `int32_t` 형 자체인가?
4. `printf("%" PRId32 "\n", value);`에서 `%`는 어디서 오는가?
5. `<stdint.h>`와 `<inttypes.h>`의 역할은 어떻게 다른가?
6. `int32_t`를 무조건 `%d`로 출력하면 왜 이식성 문제가 생길 수 있는가?

<details>
<summary>정답과 해설</summary>

1. `INT_MIN`은 `int`, `INT32_MIN`은 제공되는 `int32_t`의 경계다.
2. 아니다. 접미사 없는 정수 상수에서 적합한 정수 상수 표현을 만드는 `<stdint.h>` 함수형 매크로다.
3. 아니다. 대응 `int_least32_t`의 promoted type에 맞는다.
4. 소스의 `"%"` 문자열 조각이 제공한다. `PRId32`는 나머지 서식 조각이다.
5. `<stdint.h>`는 폭 기반 형·한계·상수 매크로를 제공하고, `<inttypes.h>`는 이를 포함·확장해 형식화된 입출력 매크로 등을 제공한다.
6. `int32_t`의 바탕형이 모든 구현에서 `int`라고 보장되지 않기 때문이다.

</details>

## 12. 핵심 정리

한계는 `INTN_MIN`, `INTN_MAX`, `UINTN_MAX` 계열로 확인하고, 소스 상수는 `INTN_C`, `UINTN_C`로 표현한다. 고정 폭 값의 직접 출력은 `<inttypes.h>`의 `PRI` 매크로를 `%`와 조합한다. 헤더와 매크로를 함께 사용해야 바탕 기본형을 추측하지 않는 코드가 된다.

## 13. 다음 Step

[Step 4-6. `uint32_t register_value`와 레지스터 모델](4-6-register-value-model.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.8 `<inttypes.h>`, 7.20 `<stdint.h>`, 특히 7.20.2~7.20.4를 확인한다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): 한계·상수 매크로를 요약한다.
- [cppreference: `PRI` format macros](https://en.cppreference.com/w/c/types/integer.html#Format_macro_constants): 형식 매크로 조합 예를 확인한다.
