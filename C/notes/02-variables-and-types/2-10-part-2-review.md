# 2-10. Part 2 종합 복습

C17 호스트 환경에서 변수 선언부터 구현별 자료형 보고서까지 연결한다. 새 제어 흐름, 배열, 포인터, 동적 메모리 할당은 도입하지 않는다. 입력 없이 초기화한 값과 구현 정보를 순서대로 출력하는 프로그램을 직접 작성한다.

## 1. 학습 목표

- 의미 있는 이름으로 변수를 선언하고 사용 전에 초기화한다.
- 정수형의 크기·부호·범위와 부동소수점형의 범위·정밀도를 구별한다.
- `sizeof`, `size_t`, `CHAR_BIT`, `<limits.h>`, `<float.h>`를 목적에 맞게 사용한다.
- 출력 서식과 인수형을 맞추고 빌드·실행·결과 해석을 각각 검토한다.
- 실제 구현의 수치를 C17의 보장과 분리하여 설명한다.

## 2. 선수 지식

Step 2-1~2-9의 종합 복습이다. 변수 이름, 선언과 초기화, 기본 정수·실수형, `sizeof`와 두 한계 헤더를 사용한다. [Step 2-9 구현별 자료형 크기 직접 확인](2-9-implementation-type-sizes.md)의 환경 보고서를 준비하면 비교하기 좋지만, 필요한 규칙은 아래에도 정리한다.

[Part 1 종합 복습](../01-program-structure/1-8-part-1-review.md)의 `<stdio.h>`, `int main(void)`, `printf`, 중괄호, 세미콜론, 주석, 성공 종료를 그대로 사용한다. 값 출력에 필요한 서식만 복습하며 입력과 복잡한 형 변환·연산은 확장하지 않는다.

## 3. 핵심 개념

### 3.1 Part 2 전체 지도

| Step | 핵심 질문 | 이번 프로그램의 증거 |
|---|---|---|
| 2-1 선언·초기화·이름 | 어떤 의미의 값을 어떤 이름으로 저장하는가? | 자료형과 이름, 첫 값, 단위 주석 |
| 2-2 `char`, `short`, `int` | 작은 정수형이 반드시 같은 크기인가? | 형별 `sizeof`, `SHRT_MIN/MAX`, `INT_MIN/MAX` |
| 2-3 `long`, `long long` | 더 높은 순위가 반드시 더 많은 byte인가? | `LONG_MIN/MAX`, `LLONG_MIN/MAX`와 크기 |
| 2-4 부호 | 음수가 필요한 값인가? | signed·unsigned 변수와 대응 범위 |
| 2-5 실수형 | 범위와 유효 정밀도가 충분한가? | `float`, `double`, `long double` 변수 |
| 2-6 크기와 단위 | C byte 수와 bit 수를 구별했는가? | `size_t`, `%zu`, `sizeof`, `CHAR_BIT` |
| 2-7 정수 한계 | 이 구현에서 어떤 값까지 가능한가? | `<limits.h>`의 매크로 |
| 2-8 실수 한계 | 큰 수를 담는 능력과 자릿수는 같은가? | `<float.h>`의 범위·정밀도 지표 |
| 2-9 구현 보고서 | 어느 환경에서 얻은 결과인가? | 컴파일러·대상·옵션·OS·CPU·ABI 기록 |

### 3.2 선언과 자료형 선택

`int item_count = 12;`는 자료형 `int`, 이름 `item_count`, 초기값 `12`를 가진 변수 정의다. 초기화와 나중의 대입은 시점이 다르다. 초기화하지 않은 지역 변수는 자동으로 0이라고 가정하지 않는다. 읽기 전에 유효한 값을 정하며, 쓰레기값을 출력하는 실험으로 학습하지 않는다.

이름은 숫자로 시작할 수 없고 공백을 포함할 수 없다. 대소문자는 구별하며 `int` 같은 키워드는 이름으로 사용할 수 없다. 초보 실습에서는 영문자로 시작하는 설명적인 이름과 밑줄을 사용하고, 예약 이름과 혼동하기 쉬운 선행 밑줄은 피한다. `x`보다 `temperature_c`처럼 의미와 단위를 드러내는 이름이 좋다.

**[C 표준]** `short`, `int`, `long`, `long long`은 `signed`를 생략해도 signed 정수형이다. 대응하는 unsigned 형의 최소값은 0이다. plain `char`, `signed char`, `unsigned char`는 서로 다른 자료형이고 plain `char`의 signedness는 구현 정의다. `char`를 무조건 signed 정수 저장용으로 선택하지 않는다.

`int`는 최소 -32767~32767, `long`은 최소 -2147483647~2147483647, `long long`은 최소 -9223372036854775807~9223372036854775807을 포함할 수 있다. 이는 **최소 보장**이며 실제 경계는 헤더에서 읽는다. 음수가 필요한 온도와 음수가 필요 없는 개수를 먼저 구별하되, unsigned가 모든 계산 문제를 해결한다고 생각하지 않는다. 범위 밖 값과 signed·unsigned 혼합 연산은 이번 실습에서 만들지 않는다.

### 3.3 크기와 값의 특성

**[C 표준]** 세 char 계열의 크기는 1 C byte다. `CHAR_BIT`는 byte당 bit 수이고 8 이상이다. `sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`이며 같은 크기도 허용된다. 대응 signed·unsigned 형은 같은 저장 크기를 가진다. `size_t`는 `sizeof` 결과의 부호 없는 정수형이지 언제나 `unsigned long`이라는 별명은 아니다.

크기가 크다는 것만으로 정수 범위나 실수 정밀도를 정확히 계산할 수 없다. 부호와 padding 등이 있기 때문이다. `float`, `double`, `long double`은 각각 실수형이며, `double`의 값 집합은 `float`를 포함하고 `long double`의 값 집합은 `double`을 포함한다. 더 넓은 형이 항상 더 큰 `sizeof`나 더 높은 정밀도를 가져야 하는 것은 아니다.

- `FLT_MIN`, `DBL_MIN`, `LDBL_MIN`: 최소 **양의 정규화** 값. 가장 음수인 값이 아니다.
- `FLT_MAX`, `DBL_MAX`, `LDBL_MAX`: 최대 유한 양의 값.
- `*_TRUE_MIN`: 비정규화 값까지 포함한 최소 양의 값. 비정규화 값을 지원하지 않으면 `*_MIN`과 같을 수 있다.
- `*_DIG`: 범위 안에서 10진수→해당 실수형→10진수 왕복 시 유지가 보장되는 10진 유효 자릿수 지표. 소수점 뒤 고정 자리 수가 아니다.
- `FLT_RADIX`, `*_MANT_DIG`: 표현의 기수와 그 기수로 센 유효 자릿수. 기수가 2일 때만 그 자릿수를 bit 수로 읽는다.
- `*_EPSILON`: 1과 그보다 큰 다음 표현 가능한 값의 차이. 모든 크기의 값에 공통인 절대 오차 한계가 아니다.

실수형의 모든 10진 소수가 정확히 저장된다고 가정하지 않는다. `%f`로 자릿수를 많이 출력한다고 변수 자체의 정밀도가 늘어나지도 않는다.

## 4. 문법

### 선언·초기화와 출력 대응

| 선언 또는 값 | 출력 서식 | 주의점 |
|---|---|---|
| `char grade = 'A';` | `%c`와 `(int)grade` | 문자로 표시한다. 기본 문자의 값은 `int`로 표현 가능하며 문자 코드 숫자가 항상 ASCII라고 가정하지 않는다. |
| `signed char adjustment = -2;` | `%hhd` | 작은 signed 정수의 십진 표시다. |
| `unsigned char level = 3;` | `%hhu` | 작은 unsigned 정수의 십진 표시다. |
| `short offset = -12;` | `%hd` | `SHRT_MIN`, `SHRT_MAX`에도 사용한다. |
| `unsigned short batches = 20;` | `%hu` | `USHRT_MAX`에도 사용한다. |
| `int count = 12;` | `%d` | `INT_MIN`, `INT_MAX`와 대응한다. |
| `unsigned int stock = 24U;` | `%u` | `UINT_MAX`와 대응한다. |
| `long distance_m = 100000L;` | `%ld` | `LONG_MIN`, `LONG_MAX`와 대응한다. |
| `unsigned long total = 100000UL;` | `%lu` | `ULONG_MAX`와 대응한다. |
| `long long ledger = 5000000000LL;` | `%lld` | `LLONG_MIN`, `LLONG_MAX`와 대응한다. |
| `unsigned long long serial = 5000000000ULL;` | `%llu` | `ULLONG_MAX`와 대응한다. |
| `float ratio = 0.5f;` | `%f` 또는 `%e` | 출력 호출에 전달될 때 `double`로 승격된다. |
| `double temperature_c = 23.5;` | `%f` 또는 `%e` | 소수 리터럴의 기본형은 `double`이다. |
| `long double estimate = 1.25L;` | `%Lf` 또는 `%Le` | 실수 리터럴의 `L`과 출력의 대문자 `L`을 확인한다. |
| `size_t bytes = sizeof(int);` | `%zu` | `<stddef.h>`를 포함한다. |

작은 정수형도 `printf` 호출 시 정수 승격을 거친다. `%hhd`·`%hhu`·`%hd`·`%hu`는 그 규칙을 반영하여 작은 형으로 해석해 출력하는 서식이다. 상세 형 변환 규칙은 뒤에서 배우며 여기서는 표의 대응을 따른다. 표의 리터럴은 각 형의 최소 보장 범위 안에 있다. 정수 리터럴의 `L`·`LL`과 소수가 있는 실수 리터럴의 `L`은 역할이 다르다.

`sizeof(자료형)` 또는 `sizeof 변수`를 사용할 수 있다. 여기서는 기본 자료형만 다루므로 측정을 위해 값을 읽거나 변경하지 않는다. 변수 값 출력과 변수 크기 출력은 별개의 작업이다.

## 5. 최소 코드 예제

선언·값·크기·한계를 연결하는 작은 예제다. 종합 과제에 필요한 모든 형을 포함한 완성 답안은 아니다. `review_probe.c`로 작성한다.

```c
#include <stdio.h>
#include <stddef.h>
#include <limits.h>
#include <float.h>

int main(void)
{
    int sample_count = 12;
    unsigned long record_count = 100000UL;
    float ratio = 0.5f;
    long double estimate = 1.25L;
    size_t count_bytes = sizeof sample_count;

    printf("sample_count=%d bytes=%zu\n", sample_count, count_bytes);
    printf("record_count=%lu ratio=%f estimate=%Lf\n",
           record_count, ratio, estimate);
    printf("CHAR_BIT=%d INT_MIN=%d INT_MAX=%d\n",
           (int)CHAR_BIT, INT_MIN, INT_MAX);
    printf("FLT_MIN=%e FLT_MAX=%e FLT_DIG=%d\n",
           FLT_MIN, FLT_MAX, FLT_DIG);
    return 0;
}
```

정상 표준 출력 환경에서는 `sample_count=12`, `record_count=100000`이 보인다. `%f`와 `%Lf`는 기본적으로 소수점 뒤 여섯 자리를 표시한다. `bytes`, `CHAR_BIT`, 한계 수치는 실행한 대상의 결과를 기록한다. `float`의 정밀도를 출력된 0의 개수로 평가하지 않는다.

## 6. 코드 해석

1. 헤더 네 개는 출력 선언, `size_t`, 정수 한계, 실수 한계를 각각 제공한다.
2. 네 값 변수는 선언할 때 초기화하며 이름으로 용도를 구분한다. 리터럴의 접미사로 의도한 형을 드러낸다.
3. `count_bytes`에는 개수 값 12가 아니라 `int` 객체 크기가 들어간다.
4. 첫 출력의 `%d`는 값, `%zu`는 크기에 대응한다. 둘을 바꾸면 안 된다.
5. 둘째 출력은 unsigned long, 승격된 float, long double을 각각 맞는 서식으로 처리한다.
6. 셋째 출력은 byte당 bit 수와 정수의 실제 경계를 확인한다. `INT_MAX`에 1을 더해 보지 않는다.
7. 마지막 출력은 실수의 범위와 10진 유효 자릿수를 분리한다. `FLT_MIN`을 가장 음수인 값으로 읽지 않는다.
8. 반환 0은 성공 상태 보고이며, 모든 종합 과제 항목을 채웠다는 자동 판정은 아니다.

## 7. 내부 동작

**[C 표준·메모리]** 선언의 자료형은 객체에 저장할 수 있는 값과 해석을 정한다. 초기화는 처음 값을 제공한다. C가 설명하는 객체를 반드시 특정 물리 RAM 칸이나 stack 위치와 동일시하지 않는다. 이 예제의 `sizeof`는 기본 자료형의 크기를 나타내는 정수 상수식이고 값 12를 검사해서 크기를 선택하지 않는다.

**[컴파일러 구현]** 컴파일러는 선언, 리터럴, 호출을 검사하고 대상에 맞는 코드를 생성한다. 최적화하면 값이 직접 출력 호출에 전달되어 별도의 메모리 저장이 없어질 수도 있다. 그래도 C 프로그램에서 요구한 값과 `sizeof`의 의미는 유지해야 한다. 경고가 없다는 사실은 프로그램의 모든 요구를 검증했다는 뜻이 아니다.

**[OS·CPU·ABI 구현]** OS는 실행 환경을 마련하고 CPU는 생성된 명령을 실행한다. ABI는 자료형 배치와 호출 규약 등을 정한다. 64-bit CPU라고 모든 정수형이 8 byte가 되지는 않는다. 같은 CPU라도 컴파일 대상·ABI에 따라 `long`이나 `long double`의 특성이 다를 수 있다. 버전, 대상, 옵션, OS, CPU 아키텍처, 확인한 ABI를 함께 남긴다.

**[동작 분류]** `sizeof(char)==1`은 표준 보장이다. plain `char`의 signedness나 구체적인 자료형 한계는 구현 정의 사항으로 문서와 연결한다. 미지정 동작은 허용된 선택의 문서화를 요구하지 않는 별도 범주이며 단순히 '미측정'이라는 뜻이 아니다. 초기화하지 않은 지역 변수 읽기, 잘못된 출력 서식, signed overflow 같은 위험한 코드를 관찰 방법으로 쓰지 않는다. 정의되지 않은 동작(UB)이 있는 실행 결과로 구현의 규칙을 추론할 수 없다.

## 8. 자주 하는 실수

- 선언만 하면 지역 변수의 값이 0이라고 생각한다. 값을 출력할 모든 변수는 초기화한다.
- `2count`나 `long`을 이름으로 쓴다. 시작 문자와 키워드를 확인한다.
- plain `char`의 signedness를 추측하거나 `char`와 `signed char`가 같은 형이라고 한다.
- `unsigned`를 '언제나 더 안전한 int'로 이해한다. 음수 필요 여부와 실제 범위를 먼저 본다.
- `sizeof`를 bit 수, 값의 크기, 현재 사용 중인 메모리 총량으로 읽는다.
- `long`의 `%ld`, `long long`의 `%lld`, `long double`의 `%Lf`를 혼동한다.
- `DBL_MIN`을 가장 음수인 값으로 읽고, `DBL_DIG`를 소수점 아래 자릿수로 읽는다.
- `long double`이 항상 `double`보다 큰 저장 공간과 높은 정밀도를 가진다고 단정한다.
- 빌드 실패 후 남아 있던 실행 파일을 실행해 이번 코드가 정상이라고 보고한다.

## 9. 필수 실습

### Part 2 값 카드와 구현 카드 작성

- **목적:** 변수 선택부터 실제 값·크기·한계 출력과 환경 해석까지 하나의 프로그램으로 연결한다.
- **해야 할 일:** `part2_review.c`를 빈 파일에서 작성한다. 문법 표의 14개 기본 자료형마다 의미 있는 변수 하나를 선언·초기화하고 값과 `sizeof`를 출력한다. `size_t` 변수에 크기 하나를 저장하여 `%zu`로 출력한다. `CHAR_BIT`와 `SCHAR_MIN/MAX`, `UCHAR_MAX`, `SHRT_MIN/MAX`, `USHRT_MAX`, `INT_MIN/MAX`, `UINT_MAX`, `LONG_MIN/MAX`, `ULONG_MAX`, `LLONG_MIN/MAX`, `ULLONG_MAX`를 출력한다. plain `char`의 범위는 `CHAR_MIN`·`CHAR_MAX`로 확인한다는 설명을 보고서에 적되 출력 과제로 추가할 필요는 없다. 세 실수형의 `*_MIN`, `*_MAX`, `*_DIG`, `*_EPSILON`과 공통 기수 `FLT_RADIX`를 출력한다. `*_MIN` 등의 별표는 매크로 계열을 뜻하는 설명 표기이므로 실제 이름으로 풀어 쓴다. 각 변수의 용도·단위와 자료형 선택 이유, 환경 정보, 표준 보장 두 개와 구현 관찰 두 개를 기록한다.
- **사용할 개념:** 이름 규칙, 선언·초기화, 14개 기본 자료형, signed·unsigned, 리터럴 접미사, `sizeof`, `size_t`, `CHAR_BIT`, 두 한계 헤더, 필요한 출력 서식.
- **예상 관찰 결과:** 초기화한 값과 형별 크기·한계가 순서대로 표시된다. 세 char 계열의 크기는 1이고 대응 signed·unsigned 형의 크기는 같다. 실수 출력은 서식에 따라 반올림될 수 있다. 구체적 경계와 byte 수는 본인의 구현 결과다.
- **확인 포인트:** 모든 값 변수가 초기화되었는가? signed·unsigned의 목적을 설명했는가? 작은 정수 한계에는 `%hhd`·`%hhu`·`%hd`·`%hu`를 맞추었는가? 크기·범위·정밀도를 별도 항목으로 기록했는가? 범위 밖 값, 조건문, 반복문, 배열, 포인터, `malloc` 없이 작성했는가?

작성 순서와 빈 표, 명령은 [실습 README](../../exercises/02-variables-and-types/2-10/README.md)에 있다. 반복되는 출력은 지금은 `printf` 문장을 직접 나열한다.

## 10. 추가 실습

- ★ **기초:** 변수 세 개의 이름과 단위를 더 명확하게 바꾸고, 값·크기가 유지되는지 새 빌드로 확인한다. 사용 전에 초기화한다는 규칙도 다시 점검한다.
- ★★ **응용:** 세 실수형의 `*_MANT_DIG`, `*_TRUE_MIN`을 추가한다. `FLT_RADIX`, 저장 byte 수, 유효 자릿수, 양의 정규화 최소값을 서로 다른 열로 정리한다.
- ★★★ **선택 도전:** 2-9 보고서와 이번 보고서를 대조한다. 환경·옵션이 같다면 공통 측정 항목이 일치하는지 확인하고, 환경이 다르면 확인한 차이만 기술한다. 다른 컴파일러 설치는 필수가 아니다.

## 11. 확인 문제

1. **오류 찾기:** `int 2count = 3;`과 `int count;`를 작성한 뒤 즉시 `count`를 출력하는 경우의 문제는 각각 무엇인가?
2. **선택·설명:** 음수가 가능한 온도용 정수와 음수가 필요 없는 재고 수에 signed·unsigned 중 무엇을 고려할 수 있는가? 최종 선택 전에 추가로 확인할 것은?
3. **예측:** `int count = 12;`에서 `sizeof count`는 12인가? 모든 C17 구현에서 4인가?
4. **서식 연결:** `size_t`, `unsigned long long`, 출력에 전달된 `float`, `long double`에 각각 맞는 서식을 하나씩 쓰라.
5. **참·거짓:** plain `char`는 항상 signed이고, `sizeof(long)`은 반드시 `sizeof(int)`보다 크다.
6. **해석:** `DBL_MIN`, `DBL_DIG`, `DBL_EPSILON`은 각각 무엇을 알려 주는가? `%f`의 표시 자릿수를 늘리면 이 값들이 바뀌는가?
7. **보고서 검토:** 빌드 성공·실행 상태 0·`long.bytes=8`을 얻었다. 이 증거만으로 과제의 모든 요구 충족과 모든 C 구현에서의 `long` 크기를 확정할 수 있는가?

<details>
<summary>정답과 해설</summary>

1. 첫째는 이름이 숫자로 시작해 문법에 맞지 않는다. 둘째는 초기화하지 않은 지역 변수를 읽으려는 문제다. 유효한 이름을 쓰고 출력 전에 초기화한다.
2. 온도에는 signed를, 재고에는 unsigned를 고려할 수 있다. 필요한 최소·최대 값이 해당 형의 범위 안인지 확인한다. unsigned 선택만으로 모든 계산이 안전해지지는 않는다.
3. 둘 다 아니다. `sizeof count`는 그 변수의 형인 `int`의 C byte 수이고 저장한 12와 무관하다. 4는 흔한 구현의 관찰이지 C17의 고정값이 아니다.
4. `%zu`, `%llu`, `%f` 또는 `%e`, `%Lf` 또는 `%Le`다. float는 출력 호출에서 double로 승격된다.
5. 둘 다 거짓이다. plain char의 signedness는 구현 정의이고 long과 int는 같은 크기일 수 있다.
6. 최소 양의 정규화 값, 보장되는 10진 유효 자릿수 지표, 1과 그보다 큰 다음 표현 가능한 값의 차이다. 출력 자릿수 설정은 자료형의 이 특성들을 바꾸지 않는다.
7. 둘 다 불가능하다. 과제의 출력 항목·값·서식을 직접 검토해야 하며 8은 그 대상과 옵션에서의 관찰이다. 다른 ABI에서는 long의 크기가 다를 수 있다.

</details>

## 12. 핵심 정리

Part 2의 완료 기준은 크기표 암기가 아니라 **값의 의미에 맞는 형을 선택하고, 초기화한 값과 구현의 한계를 올바르게 출력하고 설명하는 것**이다. 크기는 `sizeof`, byte당 bit 수는 `CHAR_BIT`, 정수 경계는 `<limits.h>`, 실수 범위·정밀도는 `<float.h>`에서 확인한다.

이름과 단위, 값과 크기, 부호와 범위, 범위와 정밀도, 표준 보장과 구현 관찰을 각각 구별한다. 빌드 성공, 실행 성공, 요구 결과 일치도 별도로 확인한다.

## 13. 다음 Step

Step 3-1. bit·byte와 자릿값

## 14. 참고 자료

- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 5.2.4.2 수치 한계, 6.2.5 자료형, 6.4.2 식별자, 6.4.4 상수, 6.5.3.4 `sizeof`, 6.7.9 초기화, 7.7·7.10 한계 헤더, 7.21.6.1 출력 서식. C11 공개 초안이며 C17 기준에서 공통인 규칙을 확인하는 공개 참고 자료다. C17 원문은 아니다.
- [GCC: C Implementation-Defined Behavior](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html): 정수·부동소수점 등 구현 정의 사항의 문서화.
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html): `-Wall`, `-Wextra`, `-Wpedantic`과 서식 진단. 경고가 없다는 것과 정확성 증명은 다르다.
