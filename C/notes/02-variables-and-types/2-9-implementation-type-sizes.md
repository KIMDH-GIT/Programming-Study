# 2-9. 구현별 자료형 크기 직접 확인

C17 호스트 환경을 기준으로 자료형의 크기·범위·정밀도를 한 장의 구현 보고서로 묶는다. 숫자를 외우는 것이 아니라 **어떤 대상과 옵션에서 얻은 값인지 함께 남기는 것**이 목표다. 다른 컴퓨터가 없어도 한 환경의 보고서로 필수 실습을 마칠 수 있다.

## 1. 학습 목표

- `sizeof`, `<limits.h>`, `<float.h>`를 함께 사용해 대상 구현의 특성을 기록한다.
- C byte 수, byte당 bit 수, 값의 범위, 유효 정밀도를 별도 항목으로 구별한다.
- 컴파일러, OS, CPU, ABI를 구별하여 관찰 조건을 적는다.
- 관찰한 `int`, `long`, `long double`의 크기를 모든 C 구현의 규칙으로 일반화하지 않는다.

## 2. 선수 지식

Step 2-1~2-8에서 배운 변수 선언·초기화, 정수형과 부동소수점형, `sizeof`와 `size_t`, 두 한계 헤더를 사용한다. 필요한 핵심은 이 노트에서도 다시 정리한다.

- 선언은 자료형과 이름을 정하고, 초기화는 처음 값을 정한다.
- `sizeof`는 객체 또는 자료형의 크기를 **C byte 단위**로 알려 준다.
- `size_t`는 `sizeof` 결과의 부호 없는 정수형 이름이다. 직접 선언할 때 `<stddef.h>`를 포함한다.
- `<limits.h>`는 정수 범위와 `CHAR_BIT`를, `<float.h>`는 부동소수점의 범위와 정밀도를 제공한다.
- [Part 1 종합 복습](../01-program-structure/1-8-part-1-review.md)의 `int main(void)`, `printf`, `return 0;`과 빌드·실행 구분을 사용한다.

서식 지정자는 이번 측정에 필요한 대응만 사용한다. 입력, 조건문, 반복문, 배열, 포인터, 동적 메모리는 필요하지 않다.

## 3. 핵심 개념

### 3.1 크기·범위·정밀도는 다른 질문이다

| 질문 | 확인 도구 | 해석 |
|---|---|---|
| 몇 C byte를 차지하는가? | `sizeof(T)` | 저장 공간의 크기이며 결과형은 `size_t`다. |
| C byte 하나에 몇 bit가 있는가? | `CHAR_BIT` | `<limits.h>`에서 확인하며 8 이상이다. |
| 정수의 최소·최대 값은? | `INT_MIN`, `INT_MAX` 등 | 해당 구현의 표현 가능한 값 범위다. |
| 실수의 양의 정규화 최소·최대 값은? | `FLT_MIN`, `FLT_MAX` 등 | `MIN`은 가장 큰 음수가 아니다. |
| 실수의 유효 자릿수는? | `FLT_DIG`, `DBL_DIG`, `LDBL_DIG` | 정해진 왕복 변환 조건에서 보장되는 10진 유효 자릿수다. |
| 1 근처의 간격은? | `FLT_EPSILON` 등 | 1과 그보다 큰 다음 표현 가능한 값의 차이다. |

**[C 표준]** `sizeof(char)`, `sizeof(signed char)`, `sizeof(unsigned char)`는 모두 1이다. `sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`이며 같은 크기도 가능하다. `sizeof(char) <= sizeof(short)`도 성립한다. 이 관계는 `int`가 반드시 4 byte라는 뜻이 아니다.

전체 저장 bit 수는 종이에 `sizeof(T)` 값과 `CHAR_BIT` 값을 곱해 계산할 수 있다. 그러나 부호나 값 이외의 padding bit가 있을 수 있으므로 그 수를 곧바로 값의 bit 수로 사용하지 않는다. 정수 범위는 한계 매크로로 확인한다. 부동소수점도 `sizeof`만으로 유효 정밀도를 계산하지 않는다.

### 3.2 무엇을 '환경'으로 기록하는가?

| 구분 | 역할 | 기록 예시와 한계 |
|---|---|---|
| 컴파일러 | 언어 규칙을 대상 코드로 번역하고 구현 선택을 제공한다. | GCC 버전, 대상 triple, 전체 옵션. 이름만 같아도 대상이 다를 수 있다. |
| OS | 실행 파일을 적재하고 실행 환경을 제공한다. | Linux 배포판·커널 또는 Windows 버전. OS 이름만으로 자료형 크기가 결정되지는 않는다. |
| CPU | 대상 명령을 실행한다. | 실행 호스트의 아키텍처와 컴파일 대상 아키텍처를 구별한다. |
| ABI | 바이너리 사이의 자료형 배치·호출 규약 등을 정한다. | 대상 ABI와 데이터 모델을 문서로 확인한다. 단순 CPU 제품명이 아니다. |
| 표준 라이브러리·헤더 | 선언, 한계 매크로, 출력 구현 등을 제공한다. | 가능하면 libc 및 개발 환경 정보도 남긴다. |

**[대표적인 ABI 사례, C 표준의 의무 아님]** 흔한 x86-64 System V LP64 환경에서 `int`는 4 byte, `long`과 `long long`은 8 byte다. Windows x64의 흔한 LLP64 환경에서는 `int`와 `long`이 4 byte, `long long`은 8 byte다. 두 이름은 포인터 크기도 포함한 데이터 모델 명칭이지만 이번에는 정수형 크기 부분만 관찰한다. 두 사례는 `CHAR_BIT == 8`인 대상의 설명이며, 모든 64-bit CPU에 적용하는 표가 아니다.

같은 CPU에서 다른 ABI의 프로그램을 실행할 수 있고, 같은 컴파일러가 다른 대상을 위한 코드를 만들 수도 있다. 두 보고서의 값이 같다는 사실도 모든 구현이 같다는 증거는 아니다.

## 4. 문법 또는 명령

### 측정에 필요한 출력 대응

| 측정 대상 | 출력 서식 | 예 |
|---|---|---|
| `sizeof(T)`, `size_t` 변수 | `%zu` | `sizeof(long)` |
| `CHAR_BIT` | `%d` | `(int)CHAR_BIT` |
| `INT_MIN`, `INT_MAX`, `FLT_DIG` 등 `int` 값 | `%d` | `INT_MAX` |
| `UINT_MAX` | `%u` | `UINT_MAX` |
| `LONG_MIN`, `LONG_MAX` | `%ld` | `LONG_MAX` |
| `ULONG_MAX` | `%lu` | `ULONG_MAX` |
| `LLONG_MIN`, `LLONG_MAX` | `%lld` | `LLONG_MAX` |
| `ULLONG_MAX` | `%llu` | `ULLONG_MAX` |
| `FLT_MIN`, `DBL_MAX` 등 | `%e` | `DBL_MAX` |
| `LDBL_MIN`, `LDBL_MAX` 등 | `%Le` | `LDBL_MAX` |

`printf`에 전달하는 `float` 값은 `double`로 승격되므로 `%e`를 쓴다. `long double`은 `%Le`를 쓴다. 이 표는 출력용이며 `scanf`에 그대로 적용하지 않는다. `%e`의 기본 출력 자릿수는 원래 값 전체를 보존하는 형식이 아니므로, 보고서의 표시된 숫자 길이로 정밀도를 추측하지 않는다.

**[GCC/Linux 구현]** 아래 명령은 셸에서 실행한다. 첫 세 명령은 프로그램의 출력이 아니라 환경 증거다.

```sh
gcc --version
gcc -dumpmachine
uname -a
gcc -std=c17 -Wall -Wextra -Wpedantic type_probe.c -o type_probe
printf 'build status=%s\n' "$?"
```

빌드 상태가 0인 경우에만 이어서 실행한다. 실패했다면 기존 실행 파일로 확인을 대신하지 않는다.

```sh
./type_probe
printf 'run status=%s\n' "$?"
```

`uname`은 실행 호스트 정보를 보여 주며 GCC의 대상 ABI를 완전히 식별하지는 못한다. `gcc -dumpmachine`도 명시한 `-m32` 같은 옵션을 모두 대신하지 못하므로 **실제 빌드 명령 전체**를 남긴다. ABI 문서를 확인하지 못했다면 보고서에 '미확인'이라고 적고 결과만으로 단정하지 않는다.

## 5. 최소 코드 예제

다음 소스를 `type_probe.c`로 작성한다. 일부 형만 측정하는 예제이며 필수 실습의 전체 보고서 답안은 아니다.

```c
#include <stdio.h>
#include <stddef.h>
#include <limits.h>
#include <float.h>

int main(void)
{
    size_t int_bytes = sizeof(int);

    printf("CHAR_BIT=%d\n", (int)CHAR_BIT);
    printf("char.bytes=%zu\n", sizeof(char));
    printf("int.bytes=%zu min=%d max=%d\n", int_bytes, INT_MIN, INT_MAX);
    printf("long.bytes=%zu min=%ld max=%ld\n",
           sizeof(long), LONG_MIN, LONG_MAX);
    printf("double.bytes=%zu min_normal=%e max=%e dig=%d epsilon=%e\n",
           sizeof(double), DBL_MIN, DBL_MAX, DBL_DIG, DBL_EPSILON);
    return 0;
}
```

**[대표적인 GCC/Linux x86-64 LP64 관찰 예]** `CHAR_BIT=8`, `char.bytes=1`, `int.bytes=4`, `long.bytes=8`처럼 나올 수 있다. 실제 실습 기록은 본인의 실행 결과로 채운다. C17의 보장은 `char.bytes=1`이며 나머지 숫자 전부를 고정하지 않는다.

## 6. 코드 해석

- `<stdio.h>`는 `printf` 선언을 제공한다. 다른 세 헤더는 크기 결과형과 한계 정보를 제공한다.
- `size_t int_bytes = sizeof(int);`는 결과를 그 결과형에 맞는 변수에 초기화한다. 별도 메모리 할당을 요청하는 문장이 아니다.
- `sizeof(char)`는 C byte의 기준을 확인한다. `CHAR_BIT`와 함께 읽어야 저장 bit 수를 이해할 수 있다.
- `int` 행은 크기와 값 범위를 나란히 보이지만 두 값을 서로 계산해 추정하지 않는다.
- `long` 행의 `%ld`는 대문자 `L`이 아니라 소문자 `l`과 `d`다. `%d`로 대체하면 안 된다.
- `double` 행의 `min_normal`은 최소 **양의 정규화 값**이다. 비정규화 값을 지원하면 더 작은 양수도 있을 수 있다. `DBL_TRUE_MIN`은 최소 양의 값을 확인하는 별도 매크로다.
- `DBL_DIG`는 유효 자릿수 지표이고, `%e`의 화면 표시 자릿수와 다르다. `DBL_EPSILON`도 모든 크기의 수에 공통인 간격은 아니다.
- `return 0;`은 성공 종료 보고다. 측정 항목과 설명이 정확하다는 자동 판정은 아니다.

## 7. 내부 동작

**[C 표준: 추상 기계·메모리]** 자료형은 값의 집합과 연산 해석을 정하고 `sizeof`는 그 형의 객체 표현 크기를 준다. 이 예제의 기본 자료형 `sizeof`는 정수 상수식이며 CPU가 실행 중 메모리를 훑어서 크기를 재는 것이 아니다. C 객체의 개념적 존재와 실제 기계의 저장 위치를 동일시하지 않는다.

**[컴파일러·헤더 구현]** 컴파일러는 대상과 옵션에 맞추어 기본 자료형 크기를 선택하고 그 구현의 표준 헤더는 한계 매크로를 제공한다. 전처리에서 매크로가 확장되고 컴파일러가 상수와 출력 호출을 번역한다. 호스트의 헤더를 임의의 다른 대상용 컴파일러와 섞어 쓰지 않는다.

**[OS·CPU·ABI 구현]** OS는 실행 파일과 라이브러리를 적재하고 CPU는 번역된 명령을 실행한다. ABI는 자료형 배치 및 인수 전달 방식 등을 맞추는 계약이다. 컴파일러가 다른 ABI를 대상으로 만들면 같은 물리 CPU라도 `long` 크기가 다를 수 있다. `printf`는 ABI에 맞추어 전달된 인수를 서식에 따라 해석하므로 서식 오류는 측정 방법 자체를 망친다.

**[보장의 경계]** 자료형의 구체적 한계와 plain `char`의 signedness 등은 구현 정의 사항으로 구현 문서와 연결한다. **미지정 동작**은 표준이 허용한 선택을 문서화하도록 요구하지 않는 경우로, '내가 아직 모르는 값'이라는 뜻이 아니다. 이번 값들을 미지정이라고 뭉뚱그리지 않는다. 서식과 인수형 불일치 같은 **정의되지 않은 동작(UB)**은 유효한 구현 측정 방법이 아니며, 한 번 그럴듯한 값이 나와도 비교 자료로 쓰지 않는다.

## 8. 자주 하는 실수

- **'내 PC는 64-bit라 모든 정수형이 8 byte다.'** CPU와 ABI를 구별하고 형별로 측정한다.
- **'GCC에서는 long이 항상 8이다.'** GCC 버전뿐 아니라 대상과 옵션을 기록한다.
- **'sizeof가 4면 값 bit가 32다.'** `CHAR_BIT`와 padding 가능성을 빼먹은 결론이다.
- **'long double이 더 크니 반드시 더 정밀하다.'** 저장 크기 대신 `LDBL_DIG`, `LDBL_MANT_DIG`, `FLT_RADIX`를 함께 본다. `long double`과 `double`이 같은 표현을 쓸 수도 있다.
- **최솟값을 계산해서 만든다.** signed 범위를 넘기는 실험 대신 헤더의 최소·최대 매크로를 읽는다.
- **비교 환경을 준비하지 못한 것을 다른 결과라고 기록한다.** 컴파일러·라이브러리 부재는 환경 준비 실패이며 자료형 관찰 결과가 아니다.
- **출력 숫자를 소스의 문자열에 직접 적는다.** 항목 이름만 문자열로 두고 수치는 `sizeof`와 매크로에서 얻는다.

## 9. 필수 실습

### 한 환경의 자료형 구현 보고서

- **목적:** 재현 가능한 명령과 실제 수치를 묶어 표준 보장과 구현 관찰을 분리한다.
- **해야 할 일:** `type_report.c`를 직접 작성한다. `char`, `signed char`, `unsigned char`, `short`, `unsigned short`, `int`, `unsigned int`, `long`, `unsigned long`, `long long`, `unsigned long long`, `float`, `double`, `long double`의 `sizeof`를 각각 출력한다. `CHAR_BIT`, `INT_MIN/MAX`, `UINT_MAX`, `LONG_MIN/MAX`, `ULONG_MAX`, `LLONG_MIN/MAX`, `ULLONG_MAX`를 출력한다. 세 실수형의 `*_MIN`, `*_MAX`, `*_DIG`, `*_EPSILON`도 출력한다. `*_MIN`처럼 쓴 표기는 설명용이며 실제로는 `FLT_MIN`, `DBL_MIN`, `LDBL_MIN` 같은 매크로 이름을 쓴다. 컴파일러 버전·대상·옵션, OS, 호스트 CPU 아키텍처, 확인한 ABI 또는 미확인 여부를 기록한다. 마지막에 '이 환경에서 관찰한 사실' 두 개와 'C17이 보장하는 사실' 두 개를 구분해 쓴다.
- **사용할 개념:** 자료형, 초기화, `sizeof`, `size_t`, `CHAR_BIT`, 정수·실수 한계 매크로, 출력 서식, 빌드·실행 상태.
- **예상 관찰 결과:** 한 프로그램에서 각 형의 C byte 수와 한계가 표시된다. 세 char 계열의 크기는 1이다. 그 밖의 구체적 숫자는 대상 구현에 따라 다르고 서로 다른 자료형의 크기가 같을 수도 있다.
- **확인 포인트:** 14개 자료형의 크기가 모두 있는가? 출력 인수와 서식이 맞는가? `MIN`에 '양의 정규화'라는 실수 해석을 붙였는가? 프로그램 출력과 환경 명령 출력을 구별했는가? 관찰을 표준의 고정값으로 쓰지 않았는가?

명령과 빈 보고서 표는 [실습 README](../../exercises/02-variables-and-types/2-9/README.md)에 있다. 조건문이나 반복문으로 자동 분류할 필요 없이 `printf` 문장을 순서대로 작성한다.

## 10. 추가 실습

- ★ **기초:** 크기가 같은 자료형 두 개를 찾고, 크기가 같아도 서로 다른 자료형이라는 설명을 붙인다. 출력에 `sizeof`와 값 범위가 별도 열로 있어야 한다.
- ★★ **응용:** `FLT_RADIX`, 세 형의 `*_MANT_DIG`, `*_TRUE_MIN`을 추가한다. 같은 byte 수만으로 같은 정밀도를 결론 내릴 수 없는 이유를 기록한다. `*_MANT_DIG`는 `FLT_RADIX`를 기수로 센 유효 자릿수이지 항상 bit 수인 것은 아니다.
- ★★★ **선택 도전:** 이미 사용 가능한 다른 컴파일러 또는 대상 환경에서 동일 소스를 빌드한다. 환경 정보와 결과를 나란히 비교하고, 달라진 조건 때문에 원인을 컴파일러 하나로 단정할 수 있는지 검토한다. 추가 도구 설치나 32-bit 지원은 필수가 아니다. 두 번째 실행이 불가능하면 미실행으로 기록한다.

## 11. 확인 문제

1. **예측:** `sizeof(char)`의 출력은 실행 전에 확정할 수 있는가? `sizeof(long)`도 같은가?
2. **계산·판단:** `sizeof(int)=4`, `CHAR_BIT=8`을 관찰했다. 전체 저장 bit 수와 값 bit 수에 대해 각각 무엇을 말할 수 있는가?
3. **오류 찾기:** `printf("%d\n", sizeof(long));`에서 잘못된 점과 수정할 서식은?
4. **보고서 평가:** 'GCC, 64-bit CPU, long=8. 따라서 C의 long은 8 byte다.'라는 결론에서 빠진 조건과 잘못된 일반화는?
5. **참·거짓:** `LDBL_MIN`은 `long double`에서 표현할 수 있는 가장 큰 음수이고, `LDBL_DIG`는 소수점 뒤 출력 자리 수다.
6. **분류:** `sizeof(char)==1`, 이 실행의 `long.bytes=8`, 잘못된 서식으로 우연히 출력된 8을 각각 어떻게 다뤄야 하는가?

<details>
<summary>정답과 해설</summary>

1. `sizeof(char)`는 1로 확정된다. `sizeof(long)`의 특정 숫자는 C17만으로 확정되지 않는다.
2. 전체 저장 공간은 32 bit다. 부호와 padding 가능성이 있으므로 값 bit 수나 정수 범위를 이 관찰만으로 단정하지 않는다.
3. `sizeof` 결과는 `size_t`다. `%zu`를 써야 한다. 우연히 맞아 보이는 출력은 올바른 호출의 증거가 아니다.
4. GCC 버전, 대상, 옵션, OS·ABI 정보가 부족하다. 숫자 8은 해당 실행 대상의 관찰이며 C의 모든 구현에 대한 규칙이 아니다.
5. 모두 거짓이다. `LDBL_MIN`은 최소 양의 정규화 값이고, `LDBL_DIG`는 10진 유효 자릿수의 보장 지표다.
6. 첫째는 C 표준 보장이다. 둘째는 해당 구현과 옵션의 관찰이다. 셋째는 잘못된 호출의 결과로, 유효한 측정 증거에서 제외하고 코드를 수정해야 한다.

</details>

## 12. 핵심 정리

보고서의 단위는 '숫자'가 아니라 **소스 + 컴파일러·대상·옵션 + 실행 환경 + 출력 + 해석**이다. `sizeof`는 C byte 수, `CHAR_BIT`는 byte당 bit 수, 한계 매크로는 값의 특성을 말한다. CPU, OS, 컴파일러, ABI를 하나의 '64-bit 환경'으로 뭉뚱그리지 않는다.

C17은 자료형 간 관계와 최소 능력을 보장하지만 흔히 관찰되는 크기표 전체를 고정하지 않는다. 실제 수치가 같든 다르든 관찰의 범위를 명시하면 유효한 보고서다.

## 13. 다음 Step

[Step 2-10. Part 2 종합 복습](2-10-part-2-review.md)

## 14. 참고 자료

- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 5.2.4.2 수치 한계, 6.2.5 자료형, 6.5.3.4 `sizeof`, 7.10 정수 한계, 7.7 부동소수점 특성, 7.21.6.1 출력 서식. C11 공개 초안이며 C17 원문이라고 부르지 않는다. 이 Step의 C17 공통 규칙을 확인하는 공개 참고 자료다.
- [GCC: C Implementation-Defined Behavior](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html): GCC의 구현 정의 사항과 대상 의존성.
- [GCC: x86 Options](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html): `-m32`, `-m64`, `-mx32` 등 대상 옵션. 이 문서를 읽는 것이 옵션을 반드시 실행하라는 뜻은 아니다.
- [x86-64 psABI 공식 저장소](https://gitlab.com/x86-psABIs/x86-64-ABI): System V 계열 x86-64 ABI 문서의 자료형 배치 규정.
- [Microsoft: Abstract Data Models](https://learn.microsoft.com/en-us/windows/win32/winprog64/abstract-data-models): Windows 64-bit 환경의 LLP64 모델.
