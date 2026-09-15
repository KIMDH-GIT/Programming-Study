# 2-7. `<limits.h>`와 정수 범위

기준은 C17 호스트 환경이다. 정수형이 담을 수 있는 값을 추측하지 않고 표준 헤더로 확인한다. 숫자가 다른 컴퓨터에서도 반드시 같다는 뜻은 아니다.

## 1. 학습 목표

- `<limits.h>`의 매크로로 정수형의 최솟값과 최댓값을 찾는다.
- C17의 최소 요구 범위와 현재 구현의 실제 범위를 구별한다.
- `CHAR_BIT`, 저장 크기, 값의 범위를 서로 다른 정보로 읽는다.
- 매크로 값의 자료형에 맞는 `printf` 서식을 사용한다.

## 2. 선수 지식

- [학습 지도 Part 2](../../C_CURRICULUM.md#part-2-변수와-자료형)의 Step 2-1~2-6: 선언·초기화, 정수형, signed·unsigned, `sizeof`와 `size_t`.
- [표준 헤더 포함](../01-program-structure/1-2-include-standard-headers.md)과 [빌드·실행](../00-compilation/0-7-build-and-run.md).
- Linux 셸과 GCC가 있는 호스트 환경을 실습 대상으로 삼는다.

필요한 숫자 출력 서식은 이 노트에서 설명한다. 조건문, 반복문, 배열 선언, 포인터 조작, 동적 할당은 사용하지 않는다. 형 변환의 일반 규칙은 Part 6에서 배우며, 여기서는 작은 unsigned형의 출력 인수를 맞추는 표기만 사용한다.

## 3. 핵심 개념

### 헤더가 알려 주는 것은 현재 구현의 값이다

**[C 표준]** `<limits.h>`는 정수형의 특성을 나타내는 매크로를 제공한다. 매크로는 이 경우 상수 식으로 사용할 수 있는 이름이며 변수가 아니다. `INT_MAX`는 실행 중 CPU에 질문하는 함수도 아니다. 표준 헤더를 포함한 뒤 그 이름을 식에서 사용한다.

**[구현 정의]** 각 구현은 표준의 최소 요구를 만족하는 실제 한계를 문서화한다. 예를 들어 C17은 모든 환경에서 `INT_MAX`가 2147483647이어야 한다고 요구하지 않는다.

아래는 **최소한 포함해야 하는 구간**이다. 실제 signed 최솟값은 표의 왼쪽 끝보다 작거나 같고, 최댓값은 오른쪽 끝보다 크거나 같을 수 있다.

| 형 | 실제 범위를 읽는 매크로 | C17이 최소한 요구하는 구간 |
|---|---|---|
| `signed char` | `SCHAR_MIN`, `SCHAR_MAX` | -127 ~ 127 |
| `unsigned char` | 0, `UCHAR_MAX` | 0 ~ 255 |
| `short` | `SHRT_MIN`, `SHRT_MAX` | -32767 ~ 32767 |
| `unsigned short` | 0, `USHRT_MAX` | 0 ~ 65535 |
| `int` | `INT_MIN`, `INT_MAX` | -32767 ~ 32767 |
| `unsigned int` | 0, `UINT_MAX` | 0 ~ 65535 |
| `long` | `LONG_MIN`, `LONG_MAX` | -2147483647 ~ 2147483647 |
| `unsigned long` | 0, `ULONG_MAX` | 0 ~ 4294967295 |
| `long long` | `LLONG_MIN`, `LLONG_MAX` | -9223372036854775807 ~ 9223372036854775807 |
| `unsigned long long` | 0, `ULLONG_MAX` | 0 ~ 18446744073709551615 |

unsigned 정수형의 최솟값은 항상 0이므로 `UINT_MIN` 같은 표준 매크로는 없다. `char`의 범위는 `CHAR_MIN`과 `CHAR_MAX`로 읽는다. plain `char`는 `signed char`, `unsigned char`와 별개의 형이며, 범위·표현은 둘 중 하나와 같다. 어느 쪽인지는 구현 정의다.

### byte 크기로 범위를 단정하지 않는다

**[C 표준]** `CHAR_BIT`는 한 C byte의 bit 수이고 최소 8이다. `sizeof(char)`는 항상 1이지만, 그 1은 1 bit나 반드시 8 bit라는 뜻이 아니다. `sizeof(int)`의 단위도 C byte다.

`sizeof(int)`와 `CHAR_BIT`로 저장 공간의 bit 수를 알 수 있어도, 일반 정수형에는 padding bit가 있을 수 있다. signed 표현도 C17에서 하나로 고정되지 않는다. 따라서 저장 공간의 bit 수만으로 `INT_MIN`을 재계산하지 말고 해당 매크로를 읽는다. 이 Step의 범위 표는 2의 보수를 전제하지 않는다.

## 4. 문법

`#include <limits.h>`를 포함하고 `INT_MIN`처럼 이름을 그대로 사용한다. `INT_MIN()`처럼 함수 호출 괄호를 붙이지 않는다.

`printf`는 서식에 따라 뒤따르는 인수를 해석한다. 서식 문자열은 인수의 자료형을 자동으로 바꾸지 않는다.

| 출력할 값 | 권장 서식·인수 | 이유 |
|---|---|---|
| `CHAR_BIT` | `%d`와 `(int)CHAR_BIT` | C17이 매크로의 형을 `int`로 고정하지 않으므로 명시적으로 변환한다. |
| `SCHAR_MIN/MAX`, `SHRT_MIN/MAX`, `INT_MIN/MAX` | `%d`와 해당 매크로 | `int` 값으로 전달된다. |
| `CHAR_MIN` | `%d`와 `(int)CHAR_MIN` | signed일 때 최솟값, unsigned일 때 0이므로 `int`로 변환해 출력한다. |
| `CHAR_MAX`, `UCHAR_MAX`, `USHRT_MAX` | `%u`와 `(unsigned int)매크로` | 작은 정수형의 승격에 따른 차이를 없앤다. |
| `UINT_MAX` | `%u` | `unsigned int`에 대응한다. |
| `LONG_MIN/MAX` | `%ld` | `long`에 대응한다. |
| `ULONG_MAX` | `%lu` | `unsigned long`에 대응한다. |
| `LLONG_MIN/MAX` | `%lld` | `long long`에 대응한다. |
| `ULLONG_MAX` | `%llu` | `unsigned long long`에 대응한다. |
| `sizeof(int)` 등 | `%zu` | `size_t`에 대응한다. |

**[C 표준]** `CHAR_BIT`와 `MB_LEN_MAX`를 제외한 이 헤더의 매크로는 대응하는 형에 정수 승격을 적용한 형을 갖는다. 예를 들어 `UCHAR_MAX`는 환경에 따라 `int`일 수도, `unsigned int`일 수도 있다. 위 표의 `(unsigned int)`는 값을 `unsigned int`형으로 명시적으로 변환하는 cast다. 이 표의 비음수 값들은 모두 `unsigned int`에 들어가므로 값은 보존된다. `CHAR_BIT`는 값이 `INT_MAX` 이하인 양의 정수 상수 식이므로 `(int)` 변환 뒤 `%d`로 안전하게 출력한다.

크기가 우연히 같아도 `long`을 `%d`로 출력해서는 안 된다. 맞지 않는 서식·인수 조합은 일반적으로 정의되지 않은 동작을 일으킬 수 있다.

## 5. 최소 코드 예제

관찰용 `limits_demo.c`로 저장할 수 있다. 필수 실습은 여기서 출력 항목을 늘려 직접 작성한다.

```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    printf("CHAR_BIT = %d\n", (int)CHAR_BIT);
    printf("char range = %d .. %u\n",
           (int)CHAR_MIN, (unsigned int)CHAR_MAX);
    printf("int range = %d .. %d\n", INT_MIN, INT_MAX);
    printf("unsigned int max = %u\n", UINT_MAX);
    printf("long range = %ld .. %ld\n", LONG_MIN, LONG_MAX);
    printf("unsigned long long max = %llu\n", ULLONG_MAX);
    return 0;
}
```

**[흔한 구현 예시, 표준 고정값 아님]** 8-bit byte, signed plain `char`, 32-bit `int`, 64-bit `long`과 `long long`을 쓰는 환경에서는 다음처럼 보일 수 있다.

```text
CHAR_BIT = 8
char range = -128 .. 127
int range = -2147483648 .. 2147483647
unsigned int max = 4294967295
long range = -9223372036854775808 .. 9223372036854775807
unsigned long long max = 18446744073709551615
```

값이 다르면 헤더·서식·빌드 환경을 먼저 확인한다. 이 예시와 다르다는 사실만으로 실패라고 판단하지 않는다.

## 6. 코드 해석

- `<limits.h>`는 한계 매크로를, `<stdio.h>`는 `printf` 선언을 제공한다. 둘은 역할이 다르다.
- 첫 호출은 byte당 bit 수를 출력한다. 정수형의 종류별 byte 크기를 출력하는 것은 아니다.
- `CHAR_MIN`이 음수면 plain `char`가 signed 쪽 범위를 갖는 구현이다. 0이면 unsigned 쪽이다. `(int)`로 출력 인수형을 맞춰도 이 값은 보존된다. 판정 코드를 쓰지 않아도 출력값을 읽어 구별할 수 있다.
- `CHAR_MAX` 앞의 cast는 출력 인수를 `%u`가 요구하는 형으로 맞춘다. 문자의 모양이 아니라 숫자 한계를 출력한다.
- `INT_MIN`, `LONG_MIN`은 각각의 형이 표현할 수 있는 가장 작은 값이다. 양의 최댓값에 임의로 부호를 붙여 계산하지 않는다.
- `%llu`의 `ll`은 `long long`, `u`는 unsigned 10진 출력을 선택한다.
- `return 0;`은 성공 종료를 보고한다. 출력값이 표준 최소 요구와 어떻게 연결되는지는 사람이 따로 점검한다.

## 7. 내부 동작

**[C 표준·번역]** 전처리 과정에서 헤더를 통해 매크로가 제공되고, 그 상수 식을 컴파일러가 해석한다. 헤더가 반드시 단순한 십진수 리터럴만으로 정의되어 있을 필요는 없다. `INT_MIN` 정의를 소스에 복사하지 말고 헤더의 이름을 사용한다.

**[메모리]** `int x = INT_MAX;`처럼 변수를 초기화하면 그 값은 `int` 범위 안이다. 그러나 매크로 자체를 읽는 것만으로 별도의 변경 가능한 변수나 메모리 공간이 생기는 것은 아니다. 변수의 물리적 저장 여부는 최적화에 따라 달라질 수 있다.

**[컴파일러·ABI]** 대상의 자료형 모델과 ABI에 맞는 값과 호출 규약으로 번역된다. 같은 CPU 계열이어도 컴파일 대상이나 ABI가 다르면 `long`의 크기·범위가 다를 수 있다. 64-bit OS라는 말만으로 모든 정수형이 64-bit라고 판단하지 않는다.

**[OS·CPU]** 이 프로그램은 OS에 현재 정수 범위를 문의하지 않는다. CPU가 제공하는 연산과 C가 허용하는 동작도 같지 않다. 특히 signed 범위를 넘는 연산은 C17에서 정의되지 않은 동작이다. `INT_MAX + 1`을 실행해서 한계를 찾는 실험은 하지 않는다. 여기서는 이미 정의된 한계를 출력하는 것으로 충분하다.

## 8. 자주 하는 실수

- `INT_MAX`를 2147483647로 하드코딩한다. 현재 구현의 값과 표준의 보장을 혼동한 것이다.
- `INT_MIN`이 반드시 `-INT_MAX - 1`이라고 가정한다. C17은 signed 표현을 2의 보수로만 제한하지 않는다.
- `-INT_MIN`을 안전한 절댓값이라고 생각한다. 그 양수가 같은 형에 들어가지 않는 구현이 있다. 실행 실습으로 사용하지 않는다.
- `CHAR_BIT`를 `sizeof(char)`와 같은 값으로 생각한다. 각각 bit/byte와 byte 단위의 정보다.
- `UINT_MIN`을 찾는다. unsigned 최솟값은 0이며 그런 표준 매크로는 없다.
- `LONG_MAX`에 `%d`, `sizeof(int)`에 `%u`를 쓴다. 각각 `%ld`, `%zu`가 필요하다.
- 작은 unsigned 매크로에 이름만 보고 `%u`를 붙인다. 승격을 고려해 위 표의 cast를 사용한다.

## 9. 필수 실습

### 실습 A. 현재 구현의 정수 한계표

- **목적:** 정수형별 한계를 표준 이름으로 관찰하고 최소 보장과 분리한다.
- **해야 할 일:** [실습 README](../../exercises/02-variables-and-types/2-7/README.md)에 따라 `integer_limits.c`를 작성한다. `CHAR_BIT`, `sizeof(int)`, `CHAR_MIN/MAX`, `SCHAR_MIN/MAX`, `UCHAR_MAX`, `SHRT_MIN/MAX`, `USHRT_MAX`, `INT_MIN/MAX`, `UINT_MAX`, `LONG_MIN/MAX`, `ULONG_MAX`, `LLONG_MIN/MAX`, `ULLONG_MAX`를 이름과 함께 출력한다. `/` 표기는 두 매크로를 모두 출력하라는 뜻이다.
- **사용할 개념:** `<limits.h>`, `<stdio.h>`, 상수 매크로, 정수 범위, `sizeof`, 해당 형의 출력 서식.
- **예상 관찰 결과:** 경고 없이 빌드되며 현재 구현의 값이 나온다. 숫자 자체는 환경에 따라 다를 수 있다.
- **확인 포인트:** 매크로 대신 숫자를 직접 쓰지 않았는가? signed 범위 밖 연산 없이 확인했는가? 표준 최소 구간과 실제 구간을 따로 기록했는가?

## 10. 추가 실습

- ★ **기초:** 필수 출력에서 `int`와 `long` 행을 골라 표준 최소 구간과 실제 구간을 나란히 적는다. 같아야 한다고 가정하지 않는다.
- ★★ **응용:** `char`의 관찰 범위가 `signed char`와 `unsigned char` 중 어느 쪽과 같은지 표를 보고 설명한다. 조건문은 쓰지 않는다.
- ★★★ **선택 도전:** GCC에서 같은 소스를 `-fsigned-char`, `-funsigned-char`로 각각 빌드한다. `CHAR_MIN/MAX`와 `SCHAR_MIN/MAX`, `UCHAR_MAX`를 비교하고 컴파일 옵션도 구현 조건임을 기록한다. 이는 GNU 옵션 비교이지 모든 C 구현의 의무 기능이 아니다.

## 11. 확인 문제

1. **빈칸:** `long long`의 최솟값은 `_____`, 최댓값은 `_____`로 읽는다.
2. **참·거짓:** C17에서 `sizeof(char) == 1`이므로 `CHAR_BIT`는 반드시 8이다.
3. **출력 해석:** 어떤 실행에서 `CHAR_MIN = 0`, `CHAR_MAX = 255`가 나왔다. plain `char`의 범위를 설명하라. 모든 구현에도 같은가?
4. **오류 찾기:** `printf("%d\n", LONG_MAX);`의 문제와 수정할 서식을 말하라.
5. **단답:** 왜 `UCHAR_MAX`를 출력할 때 `(unsigned int)`와 `%u`를 함께 쓰는가?
6. **설명:** `INT_MAX`가 32767인 구현은 이 값만을 이유로 C17 부적합인가? `INT_MAX + 1`로 다음 값을 알아봐도 되는가?

<details>
<summary>정답과 해설</summary>

1. `LLONG_MIN`, `LLONG_MAX`다. `LONG_MIN/MAX`와 다른 형의 한계다.
2. 거짓이다. `sizeof`의 단위는 C byte이며 한 byte는 최소 8 bit지만 더 클 수 있다.
3. 그 구현의 plain `char`는 unsigned 쪽 범위를 갖는다. plain `char`의 signedness와 구체적 범위는 구현에 따라 달라진다.
4. `LONG_MAX`는 `long`형인데 `%d`는 `int`를 요구한다. `%ld`로 고친다. 우연히 크기가 같아도 형을 맞춰야 한다.
5. 작은 unsigned형에 대한 정수 승격 때문에 매크로가 `int`형일 수도 있다. 값이 보존되는 cast로 출력 인수를 확실히 `unsigned int`로 맞춘다.
6. 아니다. `int`는 최소 -32767~32767을 지원하면 된다. `INT_MAX + 1`은 `int`의 표현 범위를 넘는 signed 연산이므로 정의되지 않은 동작이며 한계 관찰 방법으로 쓰지 않는다.

</details>

## 12. 핵심 정리

- `<limits.h>`는 C17의 최소 요구를 만족하는 현재 구현의 정수 한계를 알려 준다.
- `CHAR_BIT`는 byte당 bit 수다. 저장 크기와 값의 범위는 같은 정보가 아니다.
- unsigned 최솟값은 0이고 plain `char`의 범위는 별도로 확인한다.
- 출력 서식은 자료형에 맞춘다. 한계 밖 연산으로 한계를 시험하지 않는다.

## 13. 다음 Step

[2-8. `<float.h>`와 부동소수점 범위·정밀도](2-8-float-h-ranges-precision.md)에서 정수의 `MIN`과 부동소수점의 `MIN`이 왜 다른 뜻인지 배운다.

## 14. 참고 자료

- [WG14 N1570 공개 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 5.2.4.2.1 정수형의 크기와 한계, 6.2.5 정수형 관계, 6.2.6.2 정수 표현, 6.3.1.1 정수 승격, 7.10 `<limits.h>`, 7.21.6.1·7.21.6.3 출력 서식. C11 공개 초안이며 C17 원문은 아니다. 여기서 사용하는 규칙의 공개 참고 자료다.
- [GCC: Implementation-defined behavior - Integers](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html): GCC의 정수형 관련 구현 정의 사항.
- [GCC: C Dialect Options](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html): `-fsigned-char`, `-funsigned-char` 옵션의 의미.
