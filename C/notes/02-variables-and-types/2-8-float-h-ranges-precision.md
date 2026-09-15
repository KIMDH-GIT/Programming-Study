# 2-8. `<float.h>`와 부동소수점 범위·정밀도

기준은 C17 호스트 환경이다. `<float.h>`로 부동소수점형의 범위와 정밀도를 읽되, 흔한 IEEE 754 구현의 수치를 C 전체의 보장으로 오해하지 않는다.

## 1. 학습 목표

- `FLT_`, `DBL_`, `LDBL_` 접두사를 각각 `float`, `double`, `long double`과 연결한다.
- `MIN`, `MAX`, `TRUE_MIN`, `EPSILON`의 의미를 구별한다.
- 진법, 유효숫자 수, 출력 자릿수의 차이를 설명한다.
- 작은 양수를 지수 표기로 출력하고 구현별 관찰을 기록한다.

## 2. 선수 지식

- [학습 지도 Part 2](../../C_CURRICULUM.md#part-2-변수와-자료형)의 Step 2-5~2-6: 세 부동소수점형, 리터럴 접미사, `sizeof`.
- [2-7. `<limits.h>`와 정수 범위](2-7-limits-integer-ranges.md)의 상수 매크로와 구현별 한계.
- [표준 헤더](../01-program-structure/1-2-include-standard-headers.md), `main`, `printf`, GCC 빌드·실행.

필요한 출력 서식은 아래에서 설명한다. 조건문, 반복문, 배열 선언, 포인터 조작, 동적 할당은 필요 없다. 이 Step은 헤더 값을 관찰하며 복잡한 수치 알고리즘이나 입력 변환은 구현하지 않는다.

## 3. 핵심 개념

### 범위: `MIN`은 가장 음수인 값이 아니다

**[C 표준]** `<float.h>`는 부동소수점형의 특성을 나타내는 매크로를 제공한다. 아래의 `X`는 실제 이름이 아니라 `FLT`, `DBL`, `LDBL` 중 하나를 대신 쓴 표기다.

| 매크로 계열 | 의미 |
|---|---|
| `X_MAX` | 표현 가능한 가장 큰 유한 양수 |
| `X_MIN` | 표현 가능한 가장 작은 양의 **정규화된 값** |
| `X_TRUE_MIN` | 표현 가능한 가장 작은 양수; subnormal을 지원하면 `X_MIN`보다 작을 수 있다. |
| `X_EPSILON` | 해당 형에서 1보다 큰 바로 다음 표현 가능 값과 1의 차이 |
| `X_HAS_SUBNORM` | subnormal 지원 특성: -1은 판정 불가, 0은 없음, 1은 있음 |

따라서 `DBL_MIN`은 음수가 아니다. 가장 작은 유한 값, 즉 가장 음수인 값은 `-DBL_MAX`다. 0은 양수가 아니므로 `DBL_MIN`도 `DBL_TRUE_MIN`도 0을 의미하지 않는다.

**정규화(normalized)**는 유효숫자 부분의 선두 자릿수가 0이 아니도록 표현한 상태다. **비정규화 수(subnormal)**는 정규화된 최소 양수보다 0에 가까운 값을 표현하기 위해 선두의 유효 자릿수를 줄이는 값이다. 더 작은 양수를 담는 대신 그 구간에서는 상대 정밀도가 낮아질 수 있다. 모든 C 구현이 subnormal을 제공하는 것은 아니다. 제공하지 않는 형에서는 `X_TRUE_MIN`과 `X_MIN`이 같다.

### 정밀도: 진법 자릿수와 십진 자릿수를 구별한다

| 매크로 | 의미 |
|---|---|
| `FLT_RADIX` | 부동소수점 표현의 진법; 세 형에 공통이며 최소 2 |
| `FLT_MANT_DIG`, `DBL_MANT_DIG`, `LDBL_MANT_DIG` | 유효숫자 부분의 `FLT_RADIX`진 자릿수 |
| `FLT_DIG`, `DBL_DIG`, `LDBL_DIG` | 십진수 → 해당 형 → 십진수 왕복에서 보존 가능한 십진 유효 자릿수 |
| `FLT_DECIMAL_DIG`, `DBL_DECIMAL_DIG`, `LDBL_DECIMAL_DIG` | 해당 형 → 십진수 → 해당 형 왕복에 충분한 십진 유효 자릿수 |
| `DECIMAL_DIG` | 구현의 가장 넓은 지원 부동소수점형에 대한 후자의 왕복에 충분한 십진 자릿수 |

두 왕복 방향은 다르다. `X_DIG`는 십진 입력의 정밀도를, `X_DECIMAL_DIG`는 이미 저장한 값을 십진 표기로 내보낼 때 필요한 정밀도를 읽는 데 유용하다. 이는 표현 범위 안의 값을 적절히 변환한다는 전제의 특성이다. 지금 `scanf`로 왕복 프로그램을 작성할 필요는 없다.

`FLT_RADIX`가 2일 때만 `MANT_DIG`를 유효 bit 수라고 읽을 수 있다. `FLT_DIG = 6`은 소수점 아래 여섯 자리라는 뜻이 아니다. 유효숫자는 소수점 위치와 별개다. 예를 들어 123456과 0.00123456은 모두 십진 유효숫자 여섯 자리다.

`EPSILON`은 **1 근처의 간격**이다. 가장 작은 양수도 아니고 모든 크기의 값에 적용할 만능 오차 허용치도 아니다. 부동소수점 값 사이의 간격은 크기에 따라 달라진다.

### 표준 보장과 흔한 구현

**[C 표준]** `float`의 값 집합은 `double`의 부분집합이고, `double`의 값 집합은 `long double`의 부분집합이다. 세 형의 범위나 정밀도가 반드시 엄격하게 증가해야 하는 것은 아니다.

- `FLT_DIG`는 최소 6, `DBL_DIG`와 `LDBL_DIG`는 최소 10이다.
- 각 형의 `MAX`는 최소 `1E+37`, 양의 정규화된 `MIN`은 최대 `1E-37`이어야 한다. `MIN`은 더 작을수록 0에 더 가까운 정규화된 값까지 지원한다.
- `FLT_EPSILON`은 최대 `1E-5`, `DBL_EPSILON`과 `LDBL_EPSILON`은 최대 `1E-9`다.

**[흔한 IEEE 754 binary32/binary64 구현]** `float`가 binary32면 `FLT_RADIX=2`, `FLT_MANT_DIG=24`, `FLT_DIG=6`, `FLT_DECIMAL_DIG=9`가 흔하다. `double`이 binary64면 각각 유효 bit 수 53, 십진 자릿수 15와 17이다. 이 경우 `DBL_MAX`는 약 `1.797693e+308`이지만 C17이 모든 `double`에 그 범위를 요구하지는 않는다. `long double`은 `double`과 같거나 더 넓은 여러 형식일 수 있다. C17의 기본 규정만으로 IEEE 754 저장 형식, 무한대, NaN, subnormal 지원을 모두 가정하지 않는다.

## 4. 문법

`#include <float.h>`로 특성 매크로를, `#include <stdio.h>`로 출력 함수 선언을 제공받는다.

| 인수 | 사용할 출력 서식 | 주의점 |
|---|---|---|
| `FLT_RADIX`, `X_DIG`, `X_MANT_DIG`, `X_DECIMAL_DIG`, `X_HAS_SUBNORM`, `DECIMAL_DIG` | `%d` | 정수형 특성값이다. |
| `float` 값, `FLT_MIN/MAX/EPSILON/TRUE_MIN` | `%e`, `%g` | `printf`에 전달할 때 `double`로 승격된다. |
| `double` 값, `DBL_MIN/MAX/EPSILON/TRUE_MIN` | `%e`, `%g` | `double`을 받는 변환이다. |
| `long double` 값, `LDBL_MIN/MAX/EPSILON/TRUE_MIN` | `%Le`, `%Lg` | 대문자 `L`이 필요하다. |

`%e`는 지수 표기로 출력한다. `%.9e`의 9는 소수점 뒤 자릿수이므로 전체 유효숫자는 보통 10자리다. 반면 `%.9g`의 9는 유효숫자 수이며 값에 따라 고정 또는 지수 표기를 선택한다. 기본 `%f`는 소수점 뒤 여섯 자리라서 아주 작은 양수도 `0.000000`으로 보일 수 있다. 표시가 0이라고 저장값이 0인 것은 아니다.

정밀도를 상수가 아니라 인수로 주는 `%.*g`도 사용할 수 있다. `printf("%.*g\n", DBL_DECIMAL_DIG, DBL_MIN);`에서 `*`는 **포인터 조작이 아니라 서식 문법**이며, 먼저 `int` 자릿수 인수, 다음에 `double` 값 인수를 받는다. `long double`이라면 `%.*Lg`와 `LDBL_DECIMAL_DIG`를 사용한다. 출력 자릿수를 늘려도 자료형 자체의 저장 정밀도는 늘지 않는다.

## 5. 최소 코드 예제

`float_limits_demo.c`로 저장할 수 있는 관찰 예제다. 최소값, 최댓값, 정밀도를 서로 다른 항목으로 출력한다.

```c
#include <float.h>
#include <stdio.h>

int main(void)
{
    printf("FLT_RADIX = %d\n", FLT_RADIX);
    printf("float MIN = %.*g\n", FLT_DECIMAL_DIG, FLT_MIN);
    printf("float MAX = %.*g\n", FLT_DECIMAL_DIG, FLT_MAX);
    printf("float EPSILON = %.*g\n", FLT_DECIMAL_DIG, FLT_EPSILON);
    printf("float DIG / DECIMAL_DIG = %d / %d\n", FLT_DIG, FLT_DECIMAL_DIG);
    printf("double MIN = %.*g\n", DBL_DECIMAL_DIG, DBL_MIN);
    printf("double most negative finite = %.*g\n", DBL_DECIMAL_DIG, -DBL_MAX);
    printf("long double MIN = %.*Lg\n", LDBL_DECIMAL_DIG, LDBL_MIN);
    return 0;
}
```

**[흔한 구현 예시, 표준 고정값 아님]** binary32인 `float`에서는 `FLT_MIN`이 약 `1.17549435e-38`, `FLT_MAX`가 약 `3.40282347e+38`, `FLT_EPSILON`이 약 `1.19209290e-7`이다. 실제 출력의 마지막 자리와 지수는 해당 구현의 값·변환에 따른다. 이 숫자를 모든 구현의 정답으로 외우지 않는다.

## 6. 코드 해석

- 첫 호출은 저장 크기가 아니라 표현 진법을 출력한다.
- `FLT_MIN`은 양의 정규화된 최소값이고 `FLT_MAX`는 양의 유한 최댓값이다.
- `FLT_EPSILON`은 1 근처 간격이다. 이 셋은 전혀 다른 특성이다.
- `%.*g`는 두 인수를 소비한다. 먼저 출력 유효 자릿수, 다음에 값을 전달한다.
- `FLT_MIN` 등은 `float`형 특성이지만 호출에서는 `double`로 승격된다. 원래 `float`가 제공한 정밀도가 이 승격으로 늘어나는 것은 아니다.
- `-DBL_MAX`의 단항 `-`는 유한 최댓값의 부호를 바꾼다. 정수형의 `INT_MIN`과 부동소수점의 `DBL_MIN` 이름을 같은 뜻으로 읽지 않도록 대비한 줄이다.
- `LDBL_MIN`은 `long double`이므로 `%.*Lg`로 출력한다. `long double`이 `double`과 같은 크기인 구현에서도 형식은 맞춰야 한다.

## 7. 내부 동작

**[C 표준·메모리]** 부동소수점형은 유한한 정밀도로 값을 표현한다. 범위가 넓다고 그 구간의 모든 실수나 모든 정수를 표현하는 것은 아니다. `sizeof(double)`은 저장 byte 수이고 `DBL_MANT_DIG`는 진법 기준 유효 자릿수이므로 같은 정보가 아니다. 저장 공간에는 지수·부호 정보와 구현에 따라 padding도 포함될 수 있다.

**[컴파일러]** 헤더 값은 대상 구현의 특성이다. C는 일부 중간 부동소수점 계산이 원래 형보다 넓은 범위·정밀도로 평가될 수 있도록 허용하며 `FLT_EVAL_METHOD`로 평가 방식을 설명한다. 이 Step에서는 중간 계산의 한계를 실험하지 않고 형 자체의 매크로를 읽는다. 헤더의 값만으로 모든 연산이 어떤 CPU 명령 한 개로 실행된다고 결론 내리지 않는다.

**[CPU·ABI 구현]** 부동소수점 연산은 CPU 명령이나 소프트웨어 구현을 사용할 수 있다. 같은 `long double` 이름도 ABI에 따라 저장 공간과 정밀도가 달라질 수 있다. 현재 컴퓨터가 IEEE 754 형식을 흔히 사용하더라도 다른 C 구현의 형식까지 결정하지는 않는다.

**[OS·출력]** `printf`는 값을 십진 문자로 변환해 stdout에 쓴다. OS와 터미널은 그 문자를 전달·표시한다. 출력 자릿수 설정은 메모리의 원래 값을 더 정밀한 형으로 바꾸는 작업이 아니다.

## 8. 자주 하는 실수

- `DBL_MIN`을 가장 음수인 값으로 해석한다. 가장 음수인 유한 값은 `-DBL_MAX`다.
- `FLT_MIN`을 무조건 가장 작은 양수라고 한다. subnormal을 포함한 최소 양수는 `FLT_TRUE_MIN`이다.
- `FLT_EPSILON`을 가장 작은 양수나 모든 계산의 허용 오차로 사용한다. 1 근처 간격이라는 기준을 잊은 것이다.
- `FLT_MANT_DIG`를 항상 십진 자릿수나 항상 bit 수라고 읽는다. `FLT_RADIX`를 먼저 본다.
- `%.20f`가 저장 정밀도도 20자리로 늘린다고 믿는다. 표시와 저장을 구별한다.
- `long double`에 `%g`를 사용한다. `%Lg`가 필요하다.
- `double`은 반드시 binary64, `long double`은 반드시 더 정밀하다고 한다. 현재 구현의 값을 확인한다.

## 9. 필수 실습

### 실습 A. 세 부동소수점형 특성표

- **목적:** 범위·간격·자릿수를 분리하여 실제 구현을 읽는다.
- **해야 할 일:** [실습 README](../../exercises/02-variables-and-types/2-8/README.md)에 따라 `floating_limits.c`를 직접 작성한다. `FLT_RADIX`와 세 형의 `MIN`, `MAX`, `EPSILON`, `MANT_DIG`, `DIG`, `DECIMAL_DIG`를 이름과 함께 출력한다. `-DBL_MAX`도 별도 출력하고 `DBL_MIN`과 뜻을 비교한다.
- **사용할 개념:** `<float.h>`, 부동소수점형, 매크로, `%d`, `%g`·`%Lg` 또는 `%e`·`%Le`, 출력 정밀도.
- **예상 관찰 결과:** 각 `MIN`, `MAX`, `EPSILON`은 양수이고 `-DBL_MAX`는 음수다. 세 형의 차이나 동일성은 구현별 결과다.
- **확인 포인트:** 아주 작은 양수가 출력에서 0처럼 뭉개지지 않았는가? 자릿수와 범위를 별도 설명했는가? 숫자를 하드코딩하지 않았는가?

## 10. 추가 실습

- ★ **기초:** `DBL_MIN`을 `%f`와 `%e`로 각각 출력한다. 같은 값이 다르게 표시되는 이유를 적는다. 표준 최소 요구상 `DBL_MIN`은 매우 작으므로 기본 `%f`로는 0처럼 보일 수 있다.
- ★★ **응용:** 세 형의 `TRUE_MIN`과 `HAS_SUBNORM`을 출력한다. 각 `MIN`과 비교하고 -1·0·1의 의미를 사용해 관찰을 설명한다. 지원 여부를 조건문으로 판정할 필요는 없다.
- ★★★ **선택 도전:** 각 형의 `sizeof`와 `MANT_DIG`, `DIG`를 나란히 기록한다. byte 수가 더 크다고 같은 비율로 유효숫자가 늘어나는지 확인하고, 현재 관찰을 모든 ABI로 일반화할 수 없는 이유를 쓴다.

## 11. 확인 문제

1. **선택:** `DBL_MIN`은 (가장 음수인 값 / 가장 작은 양의 정규화된 값 / 0) 중 무엇인가?
2. **참·거짓:** `FLT_EPSILON`은 `float`가 표현할 수 있는 가장 작은 양수다.
3. **출력 해석:** `FLT_RADIX=2`, `FLT_MANT_DIG=24`라면 24는 어떤 단위의 유효 자릿수인가? 십진 24자리인가?
4. **오류 찾기:** `printf("%g\n", LDBL_MAX);`의 서식을 어떻게 고쳐야 하는가?
5. **비교:** `DBL_DIG`와 `DBL_DECIMAL_DIG`는 각각 어느 방향 왕복의 자릿수인가?
6. **설명:** 어떤 환경에서 `double`과 `long double`의 범위와 정밀도가 같았다. 이 사실만으로 C17 부적합인가? `DBL_MIN`을 `%f`로 출력한 0도 실제 0인가?

<details>
<summary>정답과 해설</summary>

1. 가장 작은 양의 정규화된 값이다. 가장 음수인 유한 값은 `-DBL_MAX`다. subnormal까지 포함한 가장 작은 양수는 `DBL_TRUE_MIN`이다.
2. 거짓이다. 1보다 큰 다음 `float` 값과 1의 차이다. 가장 작은 양수는 `FLT_TRUE_MIN`이다.
3. 2진 유효 자릿수, 즉 유효 bit 수 24다. 십진 24자리를 뜻하지 않는다.
4. `%Lg`로 고친다. 대문자 `L`이 `long double` 인수를 지정한다.
5. `DBL_DIG`는 십진수 → `double` → 십진수에서 보존 가능한 자릿수, `DBL_DECIMAL_DIG`는 `double` → 십진수 → `double`에 충분한 자릿수다. 변환 방향을 바꿔 읽지 않는다.
6. 아니다. 값 집합의 포함 관계는 필요하지만 반드시 더 넓어야 하지는 않는다. `%f`의 기본 소수점 뒤 여섯 자리 표시가 양수를 0처럼 보이게 할 수 있으므로 지수 표기로 확인한다.

</details>

## 12. 핵심 정리

- `MIN`은 최소 양의 정규화된 값, `TRUE_MIN`은 최소 양수, `MAX`는 유한 양의 최댓값이다.
- `EPSILON`은 1 근처 간격이며 범위나 보편적인 오차 허용치가 아니다.
- `MANT_DIG`는 진법 기준 자릿수이고 `DIG`와 `DECIMAL_DIG`는 다른 왕복 방향의 십진 자릿수다.
- 출력 자릿수, 저장 크기, 표현 정밀도를 구별한다. C17을 특정 IEEE 754 형식과 동일시하지 않는다.

## 13. 다음 Step

[학습 지도](../../C_CURRICULUM.md#part-2-변수와-자료형)의 **2-9. 구현별 자료형 크기 직접 확인**에서 크기·범위·정밀도 관찰을 모아 현재 구현의 자료형 표를 만든다.

## 14. 참고 자료

- [WG14 N1570 공개 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 5.2.4.2.2 부동소수점형의 특성, 6.2.5 형의 값 집합, 6.5.2.2 기본 인수 승격, 7.7 `<float.h>`, 7.21.6.1·7.21.6.3 출력 서식, Annex F의 IEC 60559 결합 규정. C11 공개 초안이며 C17 원문은 아니다. 본문 규칙과 선택적인 Annex F 적합성을 구별해서 읽는다.
- [GCC: Floating-point implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/Floating-point-implementation.html): GCC의 부동소수점 관련 구현 정의 사항.
- [GNU C Library: Floating Type Macros](https://www.gnu.org/software/libc/manual/html_node/Floating-Type-Macros.html): 부동소수점 범위·정밀도 매크로 설명. GNU 구현 문서는 C 표준 자체와 구별한다.
