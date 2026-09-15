# Step 2-7 실습: `<limits.h>`와 정수 범위

이론: [2-7. `<limits.h>`와 정수 범위](../../../notes/02-variables-and-types/2-7-limits-integer-ranges.md)

완성 답안 소스는 제공하지 않는다. 아래 요구에 따라 학습자가 `integer_limits.c`를 직접 작성한다.

## 준비

- C17 호스트 환경과 GCC가 설치된 Linux 셸을 사용한다.
- 저장소의 `C/`에서 아래 디렉터리로 이동한다.
- 변수와 정수형, `sizeof`, 표준 헤더, `printf`를 복습한다. 필요한 숫자 서식은 이론 노트 4절에 있다.

```sh
cd exercises/02-variables-and-types/2-7
gcc --version
```

## 필수 실습: 정수 한계표 만들기

- **목적:** 구현의 실제 정수 범위를 매크로로 확인하고 C17의 최소 보장과 구별한다.
- **해야 할 일:** `integer_limits.c`에 `<limits.h>`, `<stdio.h>`, `int main(void)`를 사용한다. 아래 출력 항목을 이름과 함께 모두 출력하고 `return 0;`으로 끝낸다. 숫자 한계를 직접 입력하지 말고 매크로를 사용한다. 출력과 구현 조건을 기록한다.
- **사용할 개념:** 한계 매크로, signed·unsigned 범위, `CHAR_BIT`, `sizeof`, `%d`, `%u`, `%ld`, `%lu`, `%lld`, `%llu`, `%zu`.
- **예상 관찰 결과:** 경고 없이 빌드되고 항목별 실제 값이 출력된다. `CHAR_BIT`는 최소 8이며 정수 범위는 노트의 표준 최소 구간을 포함한다. 특정 컴퓨터의 숫자와 일치해야 하는 과제가 아니다.
- **확인 포인트:** plain `char`의 범위를 따로 확인했는가? `CHAR_BIT`는 `(int)`로, `CHAR_MAX`, `UCHAR_MAX`, `USHRT_MAX`는 노트의 `(unsigned int)` 방식으로 인수형을 맞췄는가? `sizeof`에는 `%zu`를 썼는가? signed 범위 밖 연산을 만들지 않았는가?

### 필수 출력 항목

| 묶음 | 출력할 값 |
|---|---|
| 저장 단위 | `CHAR_BIT`, `sizeof(int)` |
| 문자형 | `CHAR_MIN`, `CHAR_MAX`, `SCHAR_MIN`, `SCHAR_MAX`, `UCHAR_MAX` |
| 짧은 정수형 | `SHRT_MIN`, `SHRT_MAX`, `USHRT_MAX` |
| 기본 정수형 | `INT_MIN`, `INT_MAX`, `UINT_MAX` |
| 긴 정수형 | `LONG_MIN`, `LONG_MAX`, `ULONG_MAX` |
| 더 긴 정수형 | `LLONG_MIN`, `LLONG_MAX`, `ULLONG_MAX` |

조건문, 반복문, 배열 선언, 포인터 조작, `malloc`은 필요 없다. `INT_MAX + 1`, `-INT_MIN` 같은 위험한 연산으로 범위를 찾지 않는다. unsigned 최솟값은 0이며 `UINT_MIN`이라는 표준 매크로는 없다.

## 빌드와 실행

```sh
gcc -std=c17 -Wall -Wextra -pedantic integer_limits.c -o integer_limits && ./integer_limits
printf 'status=%s\n' "$?"
```

`&&`는 빌드 성공 시에만 실행하는 셸 문법이다. `status`는 실행했다면 프로그램의 상태이고 빌드에서 멈췄다면 GCC의 상태다. 진단이 있으면 서식과 인수형을 확인하여 수정한 뒤 다시 빌드한다. 상태 0과 출력 요구 충족은 별도로 확인한다.

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| GCC 버전·대상 환경·추가 옵션 | |
| 빌드·실행 명령과 진단·상태 | |
| 필수 항목 전체 출력 | |
| `int`의 C17 최소 구간과 실제 구간 | |
| `long`의 C17 최소 구간과 실제 구간 | |
| plain `char`의 범위와 signedness 관찰 | |
| `CHAR_BIT`와 `sizeof(int)`의 단위 차이 | |
| C 표준 보장 한 가지 / 구현 관찰 한 가지 | |

## 추가 실습

- ★ **기초:** `int`와 `long`의 실제 범위가 표준 최소 구간을 어떻게 포함하는지 설명한다.
- ★★ **응용:** `char`의 범위를 두 문자 정수형의 범위와 비교한다. 조건문 없이 출력표를 읽어 기록한다.
- ★★★ **선택 도전:** GCC의 plain `char` 옵션에 따른 차이를 비교한다. 두 결과에서 달라진 매크로와 그대로인 매크로를 기록한다. 이 옵션은 GCC 구현 기능이다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic -fsigned-char integer_limits.c -o limits_signed_char && ./limits_signed_char
gcc -std=c17 -Wall -Wextra -pedantic -funsigned-char integer_limits.c -o limits_unsigned_char && ./limits_unsigned_char
```

## 완료 기준

- [ ] 직접 작성한 `integer_limits.c`가 모든 필수 항목을 출력한다.
- [ ] 경고 없는 빌드, 실제 출력, 종료 상태를 확인했다.
- [ ] 자료형별 출력 서식과 작은 unsigned형의 출력 cast를 설명할 수 있다.
- [ ] 표준 최소 범위와 현재 구현의 한계를 구별했다.
- [ ] 저장 bit 수만으로 일반 정수형의 값 범위를 단정하지 않았다.
- [ ] 범위 밖 연산 없이 관찰했고 후속 단원의 제어문·배열·포인터를 요구하지 않았다.

소스·명령·관찰 기록을 보존하면 된다. 제출과 커밋은 필수 조건이 아니다.
