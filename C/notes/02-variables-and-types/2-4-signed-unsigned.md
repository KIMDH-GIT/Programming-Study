# 2-4. `signed`와 `unsigned`

기준은 C17 호스트 환경이다. 부호의 유무는 음수를 출력할 수 있는지뿐 아니라 값의 범위와 산술 규칙도 결정한다. 이번 Step에서는 단순 선언·초기화·출력과 덧셈·뺄셈만으로 그 차이를 확인한다.

## 1. 학습 목표

- `signed int`와 `unsigned int`의 값 범위를 구별한다.
- 부호를 생략한 정수형 이름과 `char`의 예외를 설명한다.
- `%d`, `%u`, `%ld`, `%lu`, `%lld`, `%llu`를 실제 인수 자료형에 맞춘다.
- unsigned 모듈러 산술과 signed overflow를 혼동하지 않는다.

## 2. 선수 지식

- [Part 1 복습](../01-program-structure/1-8-part-1-review.md)의 표준 헤더, `main`, `printf`, 성공 종료.
- 변수 선언 `int count = 3;`와 초기화의 의미.
- [2-3. `long`, `long long`](2-3-long-long-long.md)의 최소 보장 범위와 구현별 차이.

`+`는 덧셈, `-`는 뺄셈이며 `-3`의 `-`는 부호를 바꾸는 단항 연산자다. 이번 실습에 필요한 의미는 여기서 설명한다. 입력, 조건문, 반복문, 배열, 포인터, 동적 메모리는 사용하지 않는다. 정수 승격·혼합 변환 전체는 Part 6에서 배운다.

## 3. 핵심 개념

### 부호 있는 형과 없는 형

**[C 표준]** signed 정수형은 음수, 0, 양수를 표현한다. unsigned 정수형은 0과 양수를 표현한다. 대응하는 signed/unsigned 정수형은 같은 저장 공간과 정렬 요구를 갖지만 값의 범위는 다르다. `unsigned`는 저장 공간을 자동으로 늘리는 기능이 아니다.

| 표기 | 같은 자료형을 나타내는 표기 | 출력 지정자 |
|---|---|---|
| `int` | `signed`, `signed int` | `%d` |
| `unsigned int` | `unsigned` | `%u` |
| `long` | `signed long`, `signed long int` | `%ld` |
| `unsigned long` | `unsigned long int` | `%lu` |
| `long long` | `signed long long`, `signed long long int` | `%lld` |
| `unsigned long long` | `unsigned long long int` | `%llu` |

`short`도 `signed short`와 같은 형이다. 단, **plain `char`는 예외**다. `char`, `signed char`, `unsigned char`는 서로 다른 세 자료형이며, plain `char`의 범위·표현이 어느 쪽과 같은지는 구현 정의다. 이번 출력 실습은 정수 승격을 별도로 다룰 필요가 없는 `int` 이상을 사용한다.

### C17에서 보장하는 범위

다음은 최소 보장 구간이다. 실제 signed 최솟값·최댓값 및 unsigned 최댓값은 `<limits.h>`로 확인한다.

| 대응 형 | signed가 적어도 표현하는 구간 | unsigned가 적어도 표현하는 구간 |
|---|---|---|
| `signed char` / `unsigned char` | -127 ~ 127 | 0 ~ 255 |
| `short` / `unsigned short` | -32767 ~ 32767 | 0 ~ 65535 |
| `int` / `unsigned int` | -32767 ~ 32767 | 0 ~ 65535 |
| `long` / `unsigned long` | -2147483647 ~ 2147483647 | 0 ~ 4294967295 |
| `long long` / `unsigned long long` | -9223372036854775807 ~ 9223372036854775807 | 0 ~ 18446744073709551615 |

**[C 표준]** unsigned 정수형에 값 표현용 비트가 N개 있으면 정확한 범위는 0 ~ 2^N - 1이다. 여기서 `^`는 수학 표기의 거듭제곱이며 C 코드의 거듭제곱 연산자가 아니다. 저장 공간에는 패딩 비트가 있을 수 있으므로 일반적으로 N을 곧바로 `sizeof(자료형) * CHAR_BIT`라고 단정하지 않는다.

signed 정수형의 부호 비트를 제외한 값 비트가 M개이면 최댓값은 2^M - 1이다. C17이 허용하는 부호와 크기 표현 또는 1의 보수에서는 최솟값이 -(2^M - 1)이다. 일반적인 2의 보수에서는 -2^M이지만, C17은 그 특수 표현을 정상 값이 아닌 트랩 표현으로 두는 구현도 허용한다. 따라서 정확한 최솟값은 한계 매크로로 확인한다. C17은 2의 보수만을 강제하지 않는다. 패딩 없는 흔한 32비트 2의 보수 구현에서는 `int`가 -2147483648 ~ 2147483647, `unsigned int`가 0 ~ 4294967295이지만 이는 특정 구현의 사례다.

### unsigned 모듈러 산술

**[C 표준]** 연산이 unsigned 자료형에서 수행될 때 수학적 결과가 범위를 벗어나면 최댓값보다 1 큰 수를 법으로 줄인 값이 된다. 즉 `unsigned int`의 법은 수학적으로 `UINT_MAX + 1`이다.

- `UINT_MAX + 1U`의 C 계산 결과는 0이다.
- `0U - 1U`의 결과는 `UINT_MAX`다.
- `UINT_MAX`의 실제 숫자가 환경마다 달라도 위 두 관계는 같다.

`U`는 unsigned 정수 상수 접미사이며 여기서 `0U`, `1U`의 자료형은 `unsigned int`다. 수학적인 법 `UINT_MAX + 1`은 `unsigned int`로 표현할 수 없다. C 식 `UINT_MAX + 1U`을 계산해 법 자체를 저장하려 하면 이미 0으로 순환한다.

**[정의되지 않은 동작]** signed 산술에서 표현 범위를 넘기는 것은 별개다. `INT_MAX + 1`은 C17이 순환 결과를 보장하지 않는 signed overflow다. 예측 가능한 음수가 된다고 기대하거나 실습의 정상 경로로 실행하지 않는다.

**변환과 연산도 구별한다.** 음수를 unsigned 자료형으로 변환하면 그 자료형의 최댓값보다 1 큰 수를 반복해서 더하거나 빼서 범위 안의 값을 얻는다. 따라서 `unsigned int converted = -1;`의 값은 `UINT_MAX`다. 음수를 저장한 것이 아니라 unsigned 범위의 값으로 변환된 것이다. 반대로 범위 밖 정수를 signed 자료형으로 변환하면 구현 정의 결과 또는 구현 정의 신호가 발생할 수 있다. 이는 signed 산술 overflow의 UB 규칙과 다르다.

주의: `unsigned char`나 `unsigned short`는 연산 전에 `int`로 승격될 수 있다. 그러므로 “unsigned 변수를 사용한 식은 전부 unsigned 연산”이라고 외우지 않는다. 이번 예제는 양쪽 피연산자를 `unsigned int`로 맞춰 이 혼동을 피한다.

## 4. 문법

| 선언 예 | 의미 |
|---|---|
| `signed int change = -3;` | 음수 변화량을 표현한다. `int`로 써도 같다. |
| `unsigned int count = 3U;` | 음수가 없는 개수를 표현한다. |
| `unsigned long budget = 4000000000UL;` | unsigned `long` 상수로 초기화한다. |
| `unsigned long long total = 10000000000ULL;` | unsigned `long long` 상수로 초기화한다. |

예시 값은 모두 해당 형의 최소 보장 범위 안이다. `UL`, `ULL`은 각각 unsigned와 길이 접미사를 함께 쓴 것이다. 접미사는 변수나 서식 문자열에 붙이지 않고 숫자에 붙인다.

`printf("count=%u\n", count);`에서 `%u`는 unsigned `int` 값을 십진수로 출력한다. `%d`를 `%u`로 바꾸는 것은 자료형 변환이 아니다. 반드시 선언한 변수의 자료형에 맞춰야 한다.

## 5. 최소 코드 예제

`signed_unsigned.c`에 저장하여 실행할 수 있다.

```c
#include <stdio.h>
#include <limits.h>

int main(void)
{
    signed int change = -3;
    unsigned int count = 3U;
    unsigned int maximum = UINT_MAX;
    unsigned int wrapped = maximum + 1U;
    unsigned int below_zero = 0U - 1U;

    printf("change=%d\n", change);
    printf("count=%u\n", count);
    printf("maximum=%u\n", maximum);
    printf("wrapped=%u\n", wrapped);
    printf("below_zero=%u\n", below_zero);
    return 0;
}
```

**[구현 사례]** `UINT_MAX`가 4294967295인 환경의 출력이다.

```text
change=-3
count=3
maximum=4294967295
wrapped=0
below_zero=4294967295
```

C17에서 변하지 않는 관찰은 `change=-3`, `count=3`, `wrapped=0`이며, `maximum`과 `below_zero`의 숫자는 항상 서로 같다. 최댓값의 십진수 자릿수 자체는 구현에 의존한다.

## 6. 코드 해석

| 구성 | 해석 |
|---|---|
| `<stdio.h>` | `printf` 선언을 제공한다. |
| `<limits.h>` | 현재 구현의 정수형 한계값 매크로를 제공한다. |
| `change`, `count` | 부호 있는 변화량과 부호 없는 개수를 구별한다. |
| `maximum = UINT_MAX` | `unsigned int`의 실제 최댓값으로 초기화한다. `UINT_MAX`는 함수 호출이 아니다. |
| `maximum + 1U` | 두 피연산자가 `unsigned int`이므로 해당 형의 법으로 계산하여 0이다. |
| `0U - 1U` | unsigned `int` 연산에서 0 바로 아래 값이 최댓값으로 순환한다. |
| 다섯 `printf` | signed 값은 `%d`, unsigned 값은 `%u`로 출력한다. |
| `return 0;` | 성공 종료한다. 산술 결과와 종료 상태는 별개다. |

## 7. 내부 동작

- **[C 표준/컴파일러]** 컴파일러는 자료형과 정수 변환 규칙으로 식의 의미를 결정한다. signed/unsigned 혼합 비교나 연산은 음수가 unsigned로 변환되는 등 의외의 결과를 만들 수 있어 이번 Step에서는 섞지 않는다.
- **[메모리]** 부호는 변수 옆에 별도의 `+` 또는 `-` 글자를 저장한다는 뜻이 아니다. 객체의 표현과 그 표현을 해석하는 자료형에 관한 규칙이다. 대응하는 signed/unsigned 형의 크기는 같지만, 음수를 unsigned로 변환하는 것을 모든 구현에서 단순 비트 복사라고 설명할 수는 없다.
- **[CPU]** 흔한 CPU는 일정 폭의 연산 결과를 만들지만 C의 의미가 CPU 명령 하나의 결과와 항상 같은 것은 아니다. unsigned의 순환은 C가 보장하고, signed overflow는 CPU가 우연히 순환하더라도 C가 보장하지 않는다.
- **[최적화]** 컴파일러는 정의된 C 동작을 보존하는 범위에서 상수 계산이나 레지스터 사용을 선택한다. signed overflow를 정상적인 값 생성 방법으로 삼으면 최적화에서도 기대가 깨질 수 있다.
- **[OS/ABI]** ABI는 정수형 크기와 호출 규약에 관여한다. OS가 unsigned 변수의 음수 입력을 자동 검증하거나 개수의 논리적 오류를 막아 주지는 않는다.

## 8. 자주 하는 실수

- **unsigned는 더 큰 정수형이다.** 대응하는 signed와 저장 크기는 같다. 표현 가능한 값의 범위가 다르다.
- **부호를 생략하면 언제나 signed다.** `char`는 예외이며 별도의 자료형이다.
- **unsigned니까 음수 대입은 반드시 오류다.** 음수의 unsigned 변환은 정의되어 있다. 경고가 나올 수는 있지만 문법 차원의 음수 입력 금지 장치가 아니다.
- **`count`가 0일 때 1을 빼도 개수이니 0이 된다.** unsigned `int` 연산에서는 `UINT_MAX`로 순환한다. 정의된 계산이라도 업무 의미상 오류일 수 있다.
- **signed 최대값에 1을 더해 순환을 확인한다.** 정의되지 않은 동작이다. 이번 예제의 unsigned 계산과 구별한다.
- **서식만 바꿔 같은 값을 signed/unsigned로 변환한다.** 출력 함수에 잘못된 자료형을 전달할 수 있다. 자료형에 맞는 서식을 유지한다.
- **작은 unsigned 형의 계산도 항상 그 폭에서 순환한다.** 정수 승격 때문에 틀릴 수 있다. 예제의 `unsigned int`를 임의로 `unsigned char`로 바꾸지 않는다.

## 9. 필수 실습

[실습 안내](../../exercises/02-variables-and-types/2-4/README.md)에 따라 다음을 수행한다.

- **목적:** signed 음수와 unsigned 경계 계산을 안전하게 관찰한다.
- **파일/해야 할 일:** `signed_unsigned.c`를 직접 작성한다. 5절의 다섯 변수와 출력 항목을 포함하고, 실행 전에 결과를 예측한다.
- **사용할 개념:** 부호 명시, 초기화, `U` 접미사, `%d`/`%u`, `UINT_MAX`, unsigned 덧셈·뺄셈.
- **예상 관찰 결과:** `change=-3`, `count=3`, `wrapped=0`이며 `maximum`과 `below_zero`는 같은 숫자다.
- **확인 포인트:** signed 경계를 넘는 식을 넣지 않았는가? 두 경계 연산의 피연산자가 `unsigned int`인가? 환경 의존 숫자와 표준 보장 관계를 구분했는가?

`C/` 디렉터리에서 다음 명령을 사용한다. `&&`는 빌드에 성공한 경우에만 실행을 연결하는 셸 문법이다.

```sh
cd exercises/02-variables-and-types/2-4
gcc -std=c17 -Wall -Wextra -Wpedantic signed_unsigned.c -o signed_unsigned && ./signed_unsigned
```

## 10. 추가 실습

- ★ **같은 양수, 다른 형:** `positive_pair.c`에서 `signed int`와 `unsigned int`에 각각 25와 25U를 넣고 올바른 서식으로 출력한다. 양의 십진수 출력이 같아도 자료형은 다름을 설명한다.
- ★★ **넓은 unsigned:** `unsigned_limits.c`에서 `ULONG_MAX`, `ULLONG_MAX`를 각각 `%lu`, `%llu`로 출력한다. `<limits.h>`를 포함하고 3절 최소 보장 구간과 실제 최댓값을 비교한다. 대응하는 signed 최댓값도 올바른 서식으로 출력해 볼 수 있다.
- ★★★ **선택 도전: 다른 폭의 순환:** `unsigned_long_wrap.c`에서 `unsigned long` 변수에 `ULONG_MAX`를 넣고 `1UL`을 더한 별도 변수를 출력한다. `0UL - 1UL`도 출력하여 결과가 각각 0과 `ULONG_MAX`인지 확인한다. `long`의 signed overflow는 실행하지 않는다.

추가 파일도 필수 실습과 동일한 C17 경고 옵션으로 빌드한다. 자료형·상수 접미사·출력 지정자를 함께 맞추는 것이 핵심이다.

## 11. 확인 문제

1. **OX:** `unsigned int`는 대응하는 `signed int`보다 항상 저장 공간이 크다.
2. **빈칸:** `unsigned long long total = 10000000000ULL;`의 올바른 십진수 출력 지정자는 무엇인가?
3. **출력 예측:** 5절에서 `UINT_MAX`가 65535인 구현이라면 `wrapped`와 `below_zero`의 값은 각각 무엇인가?
4. **선택:** `UINT_MAX + 1U`와 `INT_MAX + 1` 중 C17이 순환 결과를 보장하는 것은 무엇인가?
5. **코드 판단:** `unsigned int converted = -1;`은 음수 -1을 그대로 저장하는가? 실제 값은 무엇인가?
6. **설명:** “32비트 정수의 signed 범위는 반드시 -2147483648 ~ 2147483647이다”가 C17 전체의 설명으로 부족한 이유는 무엇인가?

<details>
<summary>정답과 해설</summary>

1. X. 대응하는 signed/unsigned 형은 같은 저장 공간과 정렬 요구를 갖는다.
2. `%llu`다. `%lld`는 signed `long long`을 위한 지정자다.
3. 각각 0과 65535다. 법은 수학적으로 65536이다.
4. `UINT_MAX + 1U`다. 결과는 0이다. `INT_MAX + 1`은 signed overflow이므로 정의되지 않은 동작이다.
5. 아니다. unsigned 변환 규칙에 따라 `UINT_MAX`가 된다. 이 규칙은 C17의 signed 표현 방식과 관계없이 성립한다.
6. 저장 비트와 값 비트를 구별해야 하고 패딩 가능성도 있다. 또한 C17은 부호와 크기 표현, 1의 보수, 2의 보수를 허용한다. 인용한 범위는 패딩 없는 32비트 2의 보수 구현의 사례이며 실제 범위는 한계값으로 확인한다.

</details>

## 12. 핵심 정리

- signed는 음수·0·양수, unsigned는 0·양수를 표현한다. plain `char`의 부호는 따로 확인한다.
- unsigned의 정확한 범위는 값 비트 N개일 때 0 ~ 2^N - 1이다.
- unsigned 자료형에서의 산술은 최댓값보다 1 큰 수를 법으로 계산한다. signed overflow는 정의되지 않은 동작이다.
- unsigned라고 논리 오류가 사라지지 않으며, 작은 정수형의 승격과 혼합 변환은 별도 규칙이다.
- 선언한 형, 정수 상수 접미사, 출력 지정자를 일치시킨다.

## 13. 다음 Step

다음은 **2-5. `float`, `double`, `long double`**이다. [커리큘럼 Part 2](../../C_CURRICULUM.md#part-2-변수와-자료형)에서 순서를 확인한다. 정수와 달리 소수 부분을 다루는 부동소수점형을 배운다.

## 14. 참고 자료

- [WG14 N2176, C17 투표 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 5.2.4.2.1 최소 정수 범위, 6.2.5 unsigned 산술, 6.2.6.2 정수 표현, 6.3.1.3 정수형 사이 변환, 6.5 표현 범위 밖 식의 결과, 7.21.6.1 출력 서식. 최종 ISO 출판본이 아닌 공개 초안이다.
- [WG14 N1570, C11 공개 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 같은 절의 보조 참고 문서다. C17 원문이나 C23 규정으로 혼동하지 않는다.
- [GCC: Integers](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html): GCC의 구현 정의 정수 표현과 변환 선택. 이 구현의 선택이 모든 C17 구현에 강제되는 것은 아니다.
