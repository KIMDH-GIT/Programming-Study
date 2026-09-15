# Step 3-1. bit·byte와 자릿값

기준은 C17 호스트 환경이다. 이 Step에서는 정수를 표현하는 가장 작은 이진 자릿값과 C가 크기를 세는 byte를 구분한다. 아직 비트 연산자, shift, 배열, 포인터는 사용하지 않는다.

## 1. 학습 목표

- bit를 binary digit, 즉 0 또는 1인 이진 자릿수로 설명한다.
- 이진수의 각 자리가 `2`의 거듭제곱 자릿값을 가진다는 사실을 계산에 사용한다.
- C byte와 정확히 8 bit인 octet을 구별한다.
- `sizeof(char) == 1`과 `CHAR_BIT >= 8`의 의미를 구별한다.
- 객체의 저장 bit 수와 값을 표현하는 bit 수가 다를 수 있음을 안다.

## 2. 선수 지식

[2-6. `sizeof`, `size_t`, byte와 `CHAR_BIT`](../02-variables-and-types/2-6-sizeof-size-t-byte-char-bit.md)에서 `sizeof`의 단위가 C byte이고 결과형이 `size_t`임을 배웠다. `<limits.h>`, `<stdio.h>`, `int main(void)`, `printf`, `%zu`, 명시적 `(int)` 변환을 사용한다.

이 Step의 이진수 표기는 계산 설명용이다. C17 소스에 `0b1011`을 쓰는 문법은 아직 없으며, 실제 C 정수 리터럴은 3-4에서 다룬다.

## 3. 핵심 개념

### bit와 이진 자릿값

**bit**는 binary digit의 줄임말이며 값은 0 또는 1이다. 십진수에서 오른쪽부터 `10⁰`, `10¹`, `10²`의 자릿값을 갖듯, 이진수에서는 오른쪽부터 `2⁰`, `2¹`, `2²`의 자릿값을 갖는다.

```text
1011₂

= 1×2³ + 0×2² + 1×2¹ + 1×2⁰
= 8 + 0 + 2 + 1
= 11₁₀
```

맨 오른쪽 자리는 `2⁰ = 1`, 그 왼쪽은 `2¹ = 2`, 다음은 4, 8, 16 순서다. 자리에 1이 있으면 그 자릿값을 더하고 0이면 더하지 않는다. 이 계산은 숫자의 **값**을 설명하며 특정 C 객체의 실제 메모리를 관찰한 것은 아니다.

### C byte는 반드시 8 bit가 아니다

**[C 표준]** C의 byte는 문자형 객체 하나를 저장할 수 있고 주소를 지정할 수 있는 저장 단위다. `sizeof`는 이 단위로 크기를 센다.

- `sizeof(char) == 1`
- `sizeof(signed char) == 1`
- `sizeof(unsigned char) == 1`
- 한 C byte의 bit 수는 `<limits.h>`의 `CHAR_BIT`로 확인한다.
- C17은 `CHAR_BIT >= 8`을 요구하지만 정확히 8로 고정하지 않는다.

정확히 8 bit인 단위는 **octet**이라고 부를 수 있다. `CHAR_BIT == 8`인 구현에서는 한 C byte와 한 octet이 같지만, 이것을 C의 보편적 정의로 바꾸면 안 된다.

**[흔한 구현 관찰]** 현대 데스크톱의 GCC 대상에서는 보통 `CHAR_BIT == 8`이다. 이는 현재 대상 구현의 선택이다.

### 저장 bit 수와 값 bit 수

`sizeof(unsigned int) * CHAR_BIT`는 `unsigned int` 객체 표현의 전체 저장 bit 수를 계산한다. 그러나 일부 정수형에는 padding bit가 있을 수 있다. 따라서 전체 저장 bit 수를 곧바로 값의 이진 자릿수라고 단정하지 않는다. 실제 범위는 `<limits.h>`의 한계 매크로로 확인한다.

## 4. 문법

| 표현 | 의미 |
|---|---|
| `sizeof(char)` | `char` 객체 크기를 C byte로 구한다. 결과는 항상 1이다. |
| `CHAR_BIT` | 한 C byte의 bit 수를 나타내는 `<limits.h>` 매크로다. |
| `(int)CHAR_BIT` | `%d`에 맞추기 위해 값을 명시적으로 `int`로 변환한다. |
| `sizeof(unsigned int)` | 구현의 `unsigned int` 객체 크기를 C byte로 구한다. |
| `%zu` | `sizeof` 결과형인 `size_t`를 출력한다. |

`sizeof(char) * CHAR_BIT`는 한 `char` 객체의 저장 bit 수다. `sizeof(char)`의 값 1과 `CHAR_BIT`의 값을 같은 단위로 읽지 않는다.

## 5. 최소 코드 예제

`bit_byte.c`에 저장한다.

```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    printf("sizeof(char)=%zu C byte\n", sizeof(char));
    printf("CHAR_BIT=%d bits per C byte\n", (int)CHAR_BIT);
    printf("sizeof(unsigned int)=%zu C bytes\n", sizeof(unsigned int));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_byte.c -o bit_byte && ./bit_byte
```

`sizeof(char)`는 1이고 `CHAR_BIT`는 8 이상이어야 한다. `unsigned int`의 구체적인 크기는 구현 관찰이다.

## 6. 코드 해석

| 코드 | 해석 |
|---|---|
| `<limits.h>` | `CHAR_BIT`를 제공한다. |
| `<stdio.h>` | `printf` 선언을 제공한다. |
| `sizeof(char)` | 한 C byte라는 표준 보장을 확인한다. |
| `(int)CHAR_BIT` | 출력 인수를 `%d`가 요구하는 형에 맞춘다. |
| `sizeof(unsigned int)` | 해당 구현의 객체 크기를 관찰한다. |
| `return 0;` | 정상 종료를 나타낸다. |

프로그램은 이진수를 문자열로 바꾸지 않는다. 저장 단위를 관찰하고, `1011₂`의 값 계산은 사람이 자릿값으로 수행한다.

## 7. 내부 동작

1. **[C 표준]** `sizeof`는 객체 표현 크기를 C byte 단위로 준다. 기본 자료형에 대한 결과는 컴파일 시 알 수 있는 정수 상수 식이다.
2. **[구현]** 컴파일 대상은 `CHAR_BIT`와 각 정수형의 크기·padding 여부를 정한다. 같은 소스라도 대상 ABI가 다르면 관찰값이 달라질 수 있다.
3. **[메모리]** 객체 표현은 값을 나타내는 bit 외에 padding을 포함할 수 있다. 숫자의 자릿값 계산과 실제 객체 표현은 구별한다.
4. **[CPU]** CPU는 이진 상태를 이용해 연산을 구현할 수 있지만 C 표준은 특정 레지스터 폭이나 회로 구성을 요구하지 않는다.
5. **[출력]** `printf`는 수치를 사람이 읽는 십진 문자로 변환한다. 화면의 글자 `8`이 메모리에 그대로 이진 숫자 하나로 저장된다는 뜻은 아니다.

## 8. 자주 하는 실수

- `sizeof(char) == 1`을 1 bit라고 읽는다.
- byte를 정의하면서 무조건 8 bit라고 쓴다.
- `CHAR_BIT == 8`인 한 실행 결과를 모든 C17 구현의 규칙으로 만든다.
- `sizeof(int) * CHAR_BIT`를 모두 값 bit라고 단정한다.
- `1011`을 C 소스에 쓰면 이진수 11이라고 생각한다. 접두사 없는 `1011`은 십진 정수 상수다.
- 이진 자릿값 그림을 실제 메모리 byte 순서 관찰로 오해한다.

## 9. 필수 실습

### 저장 단위와 자릿값 기록

- **목적:** C byte와 bit를 구별하고 이진 자릿값으로 값을 계산한다.
- **작성할 파일:** `exercises/03-integer-representation/3-1/bit_byte.c`.
- **해야 할 일:** [실습 README](../../exercises/03-integer-representation/3-1/README.md)에 따라 `sizeof(char)`, `(int)CHAR_BIT`, `sizeof(unsigned int)`를 출력한다. 이어 `1011₂`를 자릿값 식으로 풀어 11이 되는 과정을 기록한다.
- **사용할 개념:** bit, C byte, `sizeof`, `size_t`, `CHAR_BIT`, `%zu`, `%d`.
- **예상 관찰 결과:** `sizeof(char)`는 1, `CHAR_BIT`는 8 이상이다. 다른 크기는 구현에 따라 달라진다.
- **확인 포인트:** C byte 수와 bit 수를 같은 단위로 적지 않았는가?

## 10. 추가 실습

- ★ **기초:** `1101₂`의 각 자릿값을 적고 십진 값으로 바꾼다.
- ★★ **응용:** 현재 구현의 `sizeof(unsigned int) * CHAR_BIT`를 계산하고 이를 전체 저장 bit 수라고 기록한다.
- ★★★ **도전:** `CHAR_BIT == 16`, `sizeof(unsigned int) == 2`인 가상 구현의 저장 bit 수를 계산한다. 값 bit 수는 추가 정보 없이 확정하지 않는다.

## 11. 확인 문제

1. bit는 어떤 말의 줄임말이며 가능한 값은 무엇인가?
2. `1011₂`가 11인 이유를 자릿값 식으로 쓰라.
3. `sizeof(char) == 1`의 1은 어떤 단위인가?
4. C17은 `CHAR_BIT`의 정확한 값을 8로 고정하는가?
5. `sizeof(int) * CHAR_BIT`를 항상 `int`의 값 bit 수라고 해도 되는가?

<details>
<summary>정답과 해설</summary>

1. binary digit이며 0 또는 1이다.
2. `1×8 + 0×4 + 1×2 + 1×1 = 11`이다.
3. 한 C byte다.
4. 아니다. 최소 8임을 요구하며 실제 값은 구현에서 확인한다.
5. 안 된다. 전체 객체 저장 bit 수이며 padding과 부호 정보를 구별해야 한다.

</details>

## 12. 핵심 정리

- bit는 0 또는 1인 이진 자릿수다.
- 이진수는 오른쪽부터 `2⁰`, `2¹`, `2²`의 자릿값을 갖는다.
- `sizeof(char) == 1`의 단위는 C byte다.
- 한 C byte의 bit 수는 `CHAR_BIT`이며 C17에서는 8 이상이다.
- 저장 bit 수와 값 bit 수를 구별한다.

## 13. 다음 Step

[Step 3-2. 10진수와 2진수](3-2-decimal-binary.md)에서 두 진법 사이의 값 변환을 연습한다.

## 14. 참고 자료

- [WG14 N1570 공개 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 3.5 bit, 3.6 byte, 5.2.4.2.1 `CHAR_BIT`, 6.2.6 객체 표현, 6.5.3.4 `sizeof`. N1570은 C11 Committee Draft이며 C17과 공통인 규칙을 확인하는 공개 참고 자료다.
- [ISO/IEC 9899:2018](https://www.iso.org/standard/74528.html): C17 공식 표준의 서지 정보.
- [cppreference: `sizeof`](https://en.cppreference.com/w/c/language/sizeof): `sizeof` 결과와 char 계열 크기 요약.
- [GCC C Implementation](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html): 대상별 구현 선택을 확인하는 GCC 문서.
