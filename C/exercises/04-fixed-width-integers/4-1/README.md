# 4-1 실습: 고정 폭 정수형이 필요한 이유

이론: [4-1. 고정 폭 정수형이 필요한 이유](../../../notes/04-fixed-width-integers/4-1-why-fixed-width-integers.md)

## 실습 목적

일반 계산용 정수와 정확한 폭이 계약인 정수를 구별하고, `sizeof`의 C byte 단위와 `CHAR_BIT`의 bit 단위를 연결한다.

## 작성할 파일

`type_choice.c`를 빈 파일부터 직접 작성한다. 완성 답안 `.c` 파일은 제공하지 않는다.

## 해야 할 일

1. `<limits.h>`, `<stdint.h>`, `<stdio.h>`를 포함한다.
2. 일반 개수용 `int` 변수를 12로 초기화한다.
3. 외부의 정확한 32-bit 필드를 모델링하는 `uint32_t` 변수를 `UINT32_C(12)`로 초기화한다.
4. 두 값, 각 객체의 `sizeof`, `CHAR_BIT`를 출력한다.
5. 출력 기록에서 `sizeof`는 C byte, `CHAR_BIT`는 C byte당 bit 수라고 적는다.
6. 이 실습은 `uint32_t`가 제공되는 구현을 대상으로 한다고 기록한다.

## 사용할 개념

`int`, `uint32_t`, `UINT32_C`, `sizeof`, C byte, `CHAR_BIT`, exact-width 형의 선택 제공 조건.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic type_choice.c -o type_choice
```

## 실행 방법

```sh
./type_choice
```

## 예상 관찰 결과

두 변수의 값은 12다. `sizeof` 결과는 C byte 수이고 `CHAR_BIT`는 한 C byte의 bit 수다. 흔한 환경에서는 `uint32_t`가 4 C byte로 보일 수 있지만 이를 모든 C17 구현의 일반 규칙으로 쓰지 않는다.

## 확인 포인트

- 일반 계산과 외부 폭 계약에 서로 다른 형을 선택했는가?
- `%d`는 `int` 값에만 사용했는가?
- `uint32_t` 값의 출력을 이 Step의 핵심으로 만들지 않았는가?
- C byte와 bit를 같은 단위로 쓰지 않았는가?
- exact-width typedef가 선택 제공이라는 사실을 기록했는가?

## 추가 실습

- ★ **기초:** 기본 정수형 세 개의 `sizeof`를 출력하고 단위를 적는다.
- ★★ **응용:** exact-width, least-width, fast-width 계열의 선택 기준을 한 줄씩 쓴다.
- ★★★ **도전:** `sizeof(uint32_t) * CHAR_BIT`를 출력하고 그 값과 `sizeof(uint32_t)`의 단위 차이를 설명한다.

## 완료 기준

- [ ] `type_choice.c`가 지정한 C17 경고 옵션으로 진단 없이 컴파일된다.
- [ ] 프로그램이 종료 상태 0으로 실행된다.
- [ ] 일반 계산용 값과 정확한 폭 필드의 의도를 구별했다.
- [ ] `sizeof` 결과를 C byte 단위로 기록했다.
- [ ] `CHAR_BIT >= 8`과 `sizeof(char) == 1`을 올바르게 설명했다.
- [ ] `uint32_t`가 모든 C17 구현에 의무라고 쓰지 않았다.
- [ ] 완성 답안 소스를 README에 추가하지 않았다.
