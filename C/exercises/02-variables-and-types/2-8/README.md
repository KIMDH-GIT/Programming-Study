# Step 2-8 실습: `<float.h>`와 부동소수점 범위·정밀도

이론: [2-8. `<float.h>`와 부동소수점 범위·정밀도](../../../notes/02-variables-and-types/2-8-float-h-ranges-precision.md)

완성 답안 소스는 제공하지 않는다. 학습자가 `floating_limits.c`를 직접 작성하고 실제 구현의 관찰값을 기록한다.

## 준비

- C17 호스트 환경과 GCC가 설치된 Linux 셸을 사용한다.
- `float`, `double`, `long double`, 표준 헤더와 `printf`를 복습한다.
- 노트 3~4절에서 `MIN`의 의미와 출력 서식을 확인한다.
- 저장소의 `C/`에서 다음을 실행한다.

```sh
cd exercises/02-variables-and-types/2-8
gcc --version
```

## 필수 실습: 세 형의 범위·정밀도표

- **목적:** 가장 작은 양의 정규화된 값, 가장 큰 유한 값, 1 근처 간격, 유효 자릿수를 구별한다.
- **해야 할 일:** `floating_limits.c`에 `<float.h>`, `<stdio.h>`, `int main(void)`를 사용한다. 아래 항목을 이름과 함께 출력하고 `return 0;`으로 끝낸다. 매크로 대신 숫자를 하드코딩하지 않는다. `-DBL_MAX`도 별도 출력하여 `DBL_MIN`과 뜻을 비교한다.
- **사용할 개념:** 부동소수점 특성 매크로, `FLT_RADIX`, 정규화된 최소값, 유한 최댓값, epsilon, 진법 기준·십진 기준 자릿수, `%d`, `%g`·`%Lg` 또는 `%e`·`%Le`.
- **예상 관찰 결과:** 각 형의 `MIN`, `MAX`, `EPSILON`은 양수이고 `-DBL_MAX`는 음수다. 각 형의 범위와 정밀도는 같거나 다를 수 있다. 흔한 binary32/binary64 숫자와 일치하는 것은 완료 조건이 아니다.
- **확인 포인트:** `MIN`을 음수 최솟값이라고 설명하지 않았는가? `EPSILON`을 가장 작은 양수라고 쓰지 않았는가? `long double` 출력에는 대문자 `L`을 사용했는가? 유효 자릿수와 소수점 뒤 자릿수를 구별했는가?

### 필수 출력 항목

| 묶음 | 출력할 값 |
|---|---|
| 공통 진법 | `FLT_RADIX` |
| `float` 범위·간격 | `FLT_MIN`, `FLT_MAX`, `FLT_EPSILON` |
| `float` 자릿수 | `FLT_MANT_DIG`, `FLT_DIG`, `FLT_DECIMAL_DIG` |
| `double` 범위·간격 | `DBL_MIN`, `DBL_MAX`, `DBL_EPSILON`, `-DBL_MAX` |
| `double` 자릿수 | `DBL_MANT_DIG`, `DBL_DIG`, `DBL_DECIMAL_DIG` |
| `long double` 범위·간격 | `LDBL_MIN`, `LDBL_MAX`, `LDBL_EPSILON` |
| `long double` 자릿수 | `LDBL_MANT_DIG`, `LDBL_DIG`, `LDBL_DECIMAL_DIG` |

작은 양수를 0처럼 표시하지 않도록 지수 표기 또는 `%g` 계열을 사용한다. 노트의 `%.*g`·`%.*Lg` 방식은 출력 정밀도 인수에 해당 형의 `DECIMAL_DIG` 매크로를 사용할 수 있다. `*`는 서식 안에서 자릿수를 인수로 받는 표기다. 정수형 특성값은 `%d`로 출력한다.

조건문, 반복문, 배열 선언, 포인터 조작, `malloc`, `scanf`는 필요 없다. overflow나 underflow를 일부러 발생시켜 한계를 추측하지 않는다.

## 빌드와 실행

```sh
gcc -std=c17 -Wall -Wextra -pedantic floating_limits.c -o floating_limits && ./floating_limits
printf 'status=%s\n' "$?"
```

`&&`는 빌드에 성공했을 때만 실행한다. 바로 뒤 `status`는 실행했다면 프로그램, 빌드에서 멈췄다면 GCC의 상태다. 진단은 수정한 뒤 다시 빌드하고, 상태 0만으로 출력 항목과 설명이 맞다고 판단하지 않는다.

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| GCC 버전·대상 환경·추가 옵션 | |
| 빌드·실행 명령과 진단·상태 | |
| 모든 필수 항목의 실제 출력 | |
| `DBL_MIN`과 `-DBL_MAX`의 의미 차이 | |
| `EPSILON`의 기준 위치 | |
| `FLT_RADIX`와 `MANT_DIG`의 관계 | |
| `DIG`와 `DECIMAL_DIG`의 왕복 방향 차이 | |
| 세 형의 범위·정밀도가 같거나 다른 부분 | |
| C17 보장 한 가지 / 구현 관찰 한 가지 | |

## 추가 실습

- ★ **기초:** 같은 `DBL_MIN`을 `%f`와 `%e`로 출력한다. 표시가 0처럼 보여도 값이 0이 아닌 이유를 기록한다.
- ★★ **응용:** 세 형의 `TRUE_MIN`과 `HAS_SUBNORM`을 추가 출력한다. `MIN`과의 관계를 읽고 지원 상태 -1(판정 불가), 0(없음), 1(있음)을 구별한다. -1을 지원한다는 뜻으로 바꾸어 해석하지 않는다.
- ★★★ **선택 도전:** 세 형의 `sizeof`를 `%zu`로 추가 출력한다. 저장 byte 수와 `MANT_DIG`·`DIG`를 비교하여 저장 공간 전체가 유효숫자는 아님을 설명한다. 다른 환경을 사용할 수 있을 때만 같은 소스를 비교하고 환경 차이도 함께 적는다.

추가 출력은 같은 학습자 소스에 작성하고 위 명령으로 재빌드·실행하면 된다. 새 라이브러리나 입력 코드는 필요 없다.

## 완료 기준

- [ ] 직접 작성한 `floating_limits.c`가 필수 항목을 빠짐없이 출력한다.
- [ ] 경고 없는 빌드, 실제 출력, 종료 상태를 확인했다.
- [ ] `MIN`, `TRUE_MIN`, `MAX`, `EPSILON`의 의미를 구별한다.
- [ ] 작은 양수를 지수 표기나 `%g` 계열로 확인했다.
- [ ] `float` 인수의 `double` 승격과 `long double`의 `L` 서식을 구별한다.
- [ ] 자릿수 출력 설정이 저장 정밀도를 늘리지 않음을 설명한다.
- [ ] C17의 보장을 특정 IEEE 754 형식이나 현재 ABI의 수치로 바꾸어 말하지 않았다.

소스·명령·관찰 기록을 보존하면 된다. 제출과 커밋은 필수 조건이 아니다.
