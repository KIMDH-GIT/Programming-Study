# 4-3. `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

`uintN_t` 계열은 제공되는 구현에서 정확히 N bit인 unsigned 정수형이다. 0부터 2<sup>N</sup>-1까지의 범위와 정의된 모듈러 산술을 제공하지만, 프로그램의 논리 오류까지 막아 주지는 않는다.

## 1. 학습 목표

- 네 unsigned exact-width typedef의 의미와 범위를 설명한다.
- signed와 unsigned exact-width 형의 공통점과 차이를 구별한다.
- unsigned 순환이 정의된 동작임을 정확한 법과 함께 설명한다.
- `uint8_t`의 존재와 `CHAR_BIT`의 관계를 설명한다.
- exact-width unsigned 형을 외부 형식에 사용할 때의 한계를 말한다.

## 2. 선수 지식

Step 4-2의 exact-width 선택 제공 조건과 Part 3의 N-bit unsigned 범위·모듈러 산술을 사용한다. N개의 값 비트를 가진 unsigned 형은 0부터 2<sup>N</sup>-1까지 표현하고 산술은 2<sup>N</sup>을 법으로 한다.

## 3. 핵심 개념

### 3.1 unsigned exact-width typedef

| typedef | 범위 | 대응 최댓값 매크로 |
|---|---:|---|
| `uint8_t` | 0 ~ 2<sup>8</sup>-1 | `UINT8_MAX` |
| `uint16_t` | 0 ~ 2<sup>16</sup>-1 | `UINT16_MAX` |
| `uint32_t` | 0 ~ 2<sup>32</sup>-1 | `UINT32_MAX` |
| `uint64_t` | 0 ~ 2<sup>64</sup>-1 | `UINT64_MAX` |

exact-width unsigned 형은 padding bit 없이 정확한 폭을 가진다. 모든 bit가 값 표현에 참여하므로 범위를 위 표처럼 계산할 수 있다. 이 typedef들도 조건을 만족하는 형이 있을 때만 제공된다.

### 3.2 signed 대응형과의 차이

`int32_t`와 `uint32_t`가 모두 제공된다면 둘의 폭은 32 bit지만 값 해석과 범위가 다르다.

- `int32_t`: -2<sup>31</sup> ~ 2<sup>31</sup>-1
- `uint32_t`: 0 ~ 2<sup>32</sup>-1

unsigned는 음수를 표현하지 않는 대신 같은 폭에서 음이 아닌 값을 한 bit 더 넓게 표현한다. 그러나 “음수가 없으니 더 안전하다”는 결론은 틀리다. 0에서 1을 빼면 오류가 발생하는 대신 큰 값으로 순환할 수 있다.

### 3.3 정의된 순환과 업무 규칙

`uint8_t` 계산이 실제로 `uint8_t` 결과로 변환될 때 값은 256을 법으로 줄어든다. 예를 들어 255 다음은 0이고 0 바로 아래는 255다. 다만 작은 unsigned 형은 식에서 `int`로 승격될 수 있으므로 중간 계산과 저장 뒤 결과를 구별해야 한다. 정수 승격은 Part 6에서 자세히 다룬다.

정의된 순환은 언어 규칙일 뿐이다. 재고 0에서 1을 빼 큰 양수가 되는 결과는 C17상 정의되어도 업무상 잘못일 수 있다.

### 3.4 외부 형식과 byte 순서

`uint32_t`는 정확한 32-bit 값을 표현한다. 그러나 파일에 객체를 그대로 기록했을 때 byte 순서가 외부 형식과 맞는지는 별도 문제다. exact-width 선택은 폭 문제를 해결하지만 endianness, 정렬, 입출력 방법을 자동으로 해결하지 않는다.

## 4. 문법

```c
#include <stdint.h>

uint8_t byte_value = UINT8_C(255);
uint16_t channel = UINT16_C(50000);
uint32_t counter = UINT32_C(4000000000);
uint64_t total = UINT64_C(12000000000);
```

부호 없는 상수 매크로는 `UINTN_C` 형태다. 이 매크로의 정확한 결과형 규칙은 Step 4-5에서 다룬다.

## 5. 최소 코드 예제

`unsigned_widths.c`:

```c
#include <limits.h>
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    uint8_t a = UINT8_C(8);
    uint16_t b = UINT16_C(1600);
    uint32_t c = UINT32_C(320000);
    uint64_t d = UINT64_C(6400000000);

    printf("values=%llu,%llu,%llu,%llu\n",
           (unsigned long long)a, (unsigned long long)b,
           (unsigned long long)c, (unsigned long long)d);
    printf("bytes=%zu,%zu,%zu,%zu\n",
           sizeof a, sizeof b, sizeof c, sizeof d);
    printf("CHAR_BIT=%d\n", CHAR_BIT);
    return 0;
}
```

네 typedef가 모두 제공되는 구현을 대상으로 한다. 변환 후의 형이 `unsigned long long`이므로 `%llu`와 일치한다. 직접적인 이식 가능 출력은 Step 4-5의 `PRIuN` 매크로로 배운다.

## 6. 코드 해석

1. 각 선언은 양수라는 사실뿐 아니라 필요한 정확한 폭도 표현한다.
2. `UINTN_C` 매크로로 상수를 작성해 기본형 접미사를 추측하지 않는다.
3. 값은 출력 직전에 `unsigned long long`으로 변환된다.
4. `sizeof` 네 결과는 C byte 수다.
5. `uint8_t`가 제공되므로 `a`는 padding 없이 정확히 8 bit다.
6. 코드에는 범위 경계 산술이 없어 정수 승격과 저장 변환을 아직 섞지 않는다.

## 7. 내부 동작

**[C 추상 기계]** unsigned 정수형은 순수 이진 표현을 사용한다. exact-width 형은 padding이 없으므로 N개 bit가 모두 값 표현에 참여한다.

**[정수 승격]** `uint8_t`와 `uint16_t`가 `int`의 범위에 모두 들어가면 식에서 `int`로 승격될 수 있다. “변수 이름이 `uint8_t`이므로 모든 중간 연산도 8 bit”라고 말하면 안 된다.

**[컴파일러·CPU]** 작은 폭의 변수를 넓은 레지스터에서 계산할 수 있다. 소스 수준의 값 규칙과 CPU 레지스터 폭은 같은 개념이 아니다.

**[외부 시스템]** 통신 프로토콜과 파일 형식은 폭 외에도 byte 순서를 지정할 수 있다. 하드웨어 레지스터는 접근 방식까지 요구할 수 있으며 이후 Embedded/System C Part에서 다룬다.

## 8. 자주 하는 실수

- 모든 구현에 네 `uintN_t`가 있다고 단정한다.
- `uint8_t`를 C의 “byte형”이라고 일반화한다.
- unsigned 형에는 overflow 문제가 전혀 없다고 말한다.
- 작은 unsigned 형의 모든 중간 식도 같은 작은 폭으로 계산된다고 생각한다.
- `sizeof(uint64_t)`의 8을 bit 수라고 읽는다.
- `uint32_t` 객체를 그대로 파일에 쓰면 모든 시스템에서 같은 byte 순서가 된다고 생각한다.

## 9. 필수 실습

### unsigned exact-width 값과 크기 관찰

- **목적:** 네 unsigned exact-width 형의 값 계약, 저장 크기, 출력용 변환을 구별한다.
- **해야 할 일:** 네 변수를 `UINTN_C` 상수로 초기화하고 `unsigned long long`으로 변환해 출력한다. 각 `sizeof`와 `CHAR_BIT`도 출력한다.
- **사용할 개념:** `uintN_t`, `UINTN_C`, unsigned 범위, exact-width, C byte.
- **예상 관찰 결과:** 값은 초기화한 대로 보이고 흔한 환경에서 크기는 1, 2, 4, 8 C byte다.
- **확인 포인트:** 선택 제공 조건과 외부 byte 순서가 별도 문제라는 사실을 기록했는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-3/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** 8, 16, 32, 64개의 값 bit에 대한 최댓값 식을 각각 적는다.
- ★★ **응용:** `UINT8_MAX`와 0의 경계에서 저장 후 순환을 관찰하되 중간 식의 승격과 최종 변환을 따로 설명한다.
- ★★★ **도전:** 32-bit 외부 필드에 `unsigned int`보다 `uint32_t`가 의도를 더 정확히 드러내는 이유와, 여전히 남는 byte 순서 문제를 함께 쓴다.

## 11. 확인 문제

1. `uint32_t`가 제공될 때 표현 범위는 무엇인가?
2. `uint8_t`는 모든 C17 구현에서 반드시 존재하는가?
3. `uint8_t` 객체의 모든 중간 산술이 반드시 8 bit로 수행되는가?
4. `UINT32_MAX + UINT32_C(1)`의 결과를 논할 때 무엇을 먼저 확인해야 하는가?
5. 정확한 폭만 선택하면 파일의 byte 순서 문제도 해결되는가?

<details>
<summary>정답과 해설</summary>

1. 0부터 2<sup>32</sup>-1까지다.
2. 아니다. 정확히 8 bit이고 padding이 없는 unsigned 정수형이 있을 때 제공된다.
3. 아니다. 정수 승격 때문에 `int` 또는 `unsigned int`에서 계산될 수 있다.
4. 피연산자의 실제 형과 정수 승격·일반 산술 변환 뒤 계산 형을 확인해야 한다.
5. 아니다. endianness와 직렬화 규칙은 별도로 정해야 한다.

</details>

## 12. 핵심 정리

`uintN_t`는 제공될 때 정확한 N bit, padding 없음, 0부터 2<sup>N</sup>-1의 범위를 보장한다. unsigned 산술의 모듈러 규칙은 정의되어 있지만 작은 형의 정수 승격과 프로그램 요구를 함께 확인해야 한다. 폭이 맞아도 외부 byte 순서까지 자동으로 맞지는 않는다.

## 13. 다음 Step

[Step 4-4. 정확한 폭의 타입이 제공되는 조건](4-4-exact-width-availability.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.6.2, 7.20.1.1, 7.20.2.1을 확인한다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): unsigned exact-width 형과 한계 매크로를 요약한다.
- [GCC: C implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html): GCC 대상별 구현 선택을 확인하는 출발점이다.
