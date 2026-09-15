# 2-4 실습: `signed`와 `unsigned`

이론: [2-4. `signed`와 `unsigned`](../../../notes/02-variables-and-types/2-4-signed-unsigned.md)

완성 답안 소스는 제공하지 않는다. 각 파일은 학습자가 직접 작성한다. C17 호스트 환경과 GCC/Linux 셸을 기준으로 하며 입력, 조건문, 반복문, 배열, 포인터, 동적 메모리는 사용하지 않는다.

## 준비와 작성 파일

저장소의 `C/`에서 다음 디렉터리로 이동한다.

```sh
cd exercises/02-variables-and-types/2-4
gcc --version
```

| 파일 | 구분 | 역할 |
|---|---|---|
| `signed_unsigned.c` | 필수 | signed 음수와 unsigned 경계 계산 출력 |
| `positive_pair.c` | 추가 ★ | 같은 양수를 서로 다른 형으로 출력 |
| `unsigned_limits.c` | 추가 ★★ | 넓은 unsigned 한계값 관찰 |
| `unsigned_long_wrap.c` | 추가 ★★★ | unsigned `long`의 순환 관찰 |

각 파일에 독립적인 `int main(void)`를 작성하고 `return 0;`으로 끝낸다. 출력에는 `<stdio.h>`, 한계값 매크로에는 `<limits.h>`가 필요하다.

## 필수 과제: 부호와 경계 계산

- **목적:** signed의 음수 표현과 unsigned의 모듈러 산술을 구별한다.
- **파일:** `signed_unsigned.c`.
- **해야 할 일:**
  1. `signed int` 변수 `change`를 -3으로, `unsigned int` 변수 `count`를 3U로 초기화한다.
  2. `unsigned int` 변수 `maximum`을 `UINT_MAX`로 초기화한다.
  3. `unsigned int` 변수 `wrapped`를 `maximum + 1U`로 초기화한다.
  4. `unsigned int` 변수 `below_zero`를 `0U - 1U`로 초기화한다.
  5. 위 순서대로 변수 이름과 값을 다섯 줄로 출력한다. `change`에는 `%d`, 나머지에는 `%u`를 사용한다.
  6. 실행 전에 `wrapped`와 `below_zero`를 예측하고 실행 결과와 비교한다.
- **사용할 개념:** 부호 지정, 초기화, unsigned 상수 `U`, `UINT_MAX`, `%d`/`%u`, 덧셈·뺄셈, 모듈러 산술.
- **확인 포인트:** 두 경계 계산이 모두 `unsigned int`에서 수행되는가? unsigned 변수를 `unsigned char`로 바꾸거나 signed 최대값을 넘기는 실험을 추가하지 않았는가?

## 컴파일과 실행

현재 디렉터리에서 다음을 실행한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_unsigned.c -o signed_unsigned && ./signed_unsigned
printf 'status=%s\n' "$?"
```

`&&`는 빌드 성공 뒤에만 실행하는 셸 문법이다. `status`는 실행했다면 프로그램 상태, 빌드에서 멈췄다면 빌드 실패 상태다. 경고가 있으면 서식과 자료형부터 확인하고 고친 뒤 다시 빌드한다. signed overflow를 재현해서 경고 유무나 실행 결과로 안전성을 판단하지 않는다.

## 예상 관찰 결과

항상 성립하는 결과는 다음과 같다.

| 출력 항목 | 기대값 또는 관계 |
|---|---|
| `change` | -3 |
| `count` | 3 |
| `maximum` | 현재 구현의 `UINT_MAX` |
| `wrapped` | 0 |
| `below_zero` | `maximum`과 같은 값 |

예를 들어 `UINT_MAX`가 4294967295인 구현에서는 다음과 같다. 이 숫자 자체를 모든 환경의 완료 조건으로 삼지 않는다.

```text
change=-3
count=3
maximum=4294967295
wrapped=0
below_zero=4294967295
```

`wrapped`가 0이 되는 것은 정의된 unsigned 산술이다. 그렇다고 재고 수량 0에서 1을 빼 큰 양수가 되는 것이 업무상 올바른 결과라는 뜻은 아니다. signed의 `INT_MAX + 1`은 같은 순환 규칙이 아니라 정의되지 않은 동작이다.

## 기록과 점검

| 항목 | 직접 기록 |
|---|---|
| GCC 버전과 대상 환경 | |
| 빌드 명령, 경고 유무, 종료 상태 | |
| 실행 전 두 경계값 예측 | |
| 실제 다섯 줄 출력 | |
| 표준이 보장하는 결과 관계 | |
| 현재 구현에서 관찰한 `UINT_MAX` | |
| 정의된 unsigned 산술과 논리적 개수 오류의 차이 | |

추가 점검:

- `unsigned`와 `unsigned int`는 같은 자료형이다.
- 대응하는 signed/unsigned 형은 같은 저장 공간을 사용한다.
- plain `char`는 `signed char`와 동일한 자료형이 아니다.
- 음수의 unsigned 변환은 음수 입력을 거부하는 기능이 아니다.
- 수학적인 법 `UINT_MAX + 1`과 C 계산 `UINT_MAX + 1U`의 결과 0을 구별한다.

## 추가 실습

- ★ **`positive_pair.c`:** `signed int`와 `unsigned int`에 각각 25와 25U를 넣고 `%d`, `%u`로 출력한다. 숫자 출력이 같아도 자료형이 같다는 뜻은 아님을 적는다.
- ★★ **`unsigned_limits.c`:** `ULONG_MAX`, `ULLONG_MAX`를 각각 `%lu`, `%llu`로 출력한다. 이 값들이 적어도 4294967295, 18446744073709551615인지 기록으로 비교한다. 조건문은 필요 없다. 선택으로 `LONG_MAX`, `LLONG_MAX`도 각각 `%ld`, `%lld`로 출력한다.
- ★★★ **선택 도전, `unsigned_long_wrap.c`:** `unsigned long` 변수에 `ULONG_MAX`를 넣고 `1UL`을 더한 별도 변수를 출력한다. `0UL - 1UL`의 값도 출력한다. 결과가 각각 0과 `ULONG_MAX`인지 확인하고 `long`의 바이트 수를 가정할 필요가 없는 이유를 적는다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic positive_pair.c -o positive_pair && ./positive_pair
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_limits.c -o unsigned_limits && ./unsigned_limits
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_long_wrap.c -o unsigned_long_wrap && ./unsigned_long_wrap
```

## 완료 기준

- [ ] `signed_unsigned.c`를 직접 작성하고 C17 경고 옵션으로 경고 없이 빌드·실행했다.
- [ ] 다섯 출력 항목과 성공 종료를 확인했다.
- [ ] `wrapped`는 0, `below_zero`는 `maximum`과 같음을 확인했다.
- [ ] signed 값과 unsigned 값의 출력 지정자를 맞췄다.
- [ ] `UINT_MAX`의 실제 숫자는 구현 관찰이고 순환 관계는 표준 보장임을 구별한다.
- [ ] signed overflow를 실행하거나 unsigned의 정의된 결과를 무조건 논리적으로 올바르다고 주장하지 않았다.

추가 실습과 커밋은 필수 완료 조건이 아니다.
