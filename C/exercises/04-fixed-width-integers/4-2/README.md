# 4-2 실습: signed exact-width 정수형

이론: [4-2. `int8_t`, `int16_t`, `int32_t`, `int64_t`](../../../notes/04-fixed-width-integers/4-2-signed-exact-width-integers.md)

## 실습 목적

네 signed exact-width 형의 값과 저장 크기를 관찰하고, 정확한 bit 폭과 `sizeof`의 C byte 단위를 구별한다.

## 작성할 파일

`signed_widths.c`

## 해야 할 일

1. `<limits.h>`, `<stdint.h>`, `<stdio.h>`를 포함한다.
2. `int8_t`, `int16_t`, `int32_t`, `int64_t` 변수를 각각 음수인 `INTN_C` 상수로 초기화한다.
3. 네 값을 `long long`으로 변환해 `%lld`로 출력한다.
4. 네 객체의 `sizeof`와 `CHAR_BIT`를 출력한다.
5. exact-width signed 형의 제공 조건과 2의 보수 조건을 기록한다.
6. 범위를 넘는 signed 산술은 작성하지 않는다.

## 사용할 개념

signed exact-width typedef, `INTN_C`, 2의 보수, padding 없음, `sizeof`, C byte, `CHAR_BIT`.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_widths.c -o signed_widths
```

## 실행 방법

```sh
./signed_widths
```

## 예상 관찰 결과

초기화한 네 음수가 그대로 출력된다. 흔한 환경에서는 객체 크기가 1, 2, 4, 8 C byte지만, 기록에는 현재 구현의 관찰이라고 표시한다.

## 확인 포인트

- 모든 상수가 대응하는 `INTN_C` 매크로를 사용하는가?
- `%lld`에 전달하기 전에 `long long`으로 변환했는가?
- exact-width 형이 padding bit를 갖지 않는다고 설명했는가?
- `int8_t`의 존재와 `CHAR_BIT`의 관계를 올바르게 설명했는가?
- signed overflow 실험을 넣지 않았는가?

## 추가 실습

- ★ **기초:** 네 형의 최솟값과 최댓값 매크로 이름을 표로 작성한다.
- ★★ **응용:** `sizeof`와 `CHAR_BIT`로 각 객체의 저장 bit 수를 계산한다.
- ★★★ **도전:** `int32_t`와 `int`의 크기가 같은 구현에서도 두 선언의 계약이 다른 이유를 쓴다.

## 완료 기준

- [ ] 네 exact-width typedef가 제공되는 환경에서 진단 없이 컴파일된다.
- [ ] 실행 상태가 0이다.
- [ ] 네 값과 네 `sizeof` 결과를 기록했다.
- [ ] C byte와 bit를 구별했다.
- [ ] 선택 제공과 2의 보수 조건을 설명했다.
- [ ] 범위를 넘는 signed 산술을 실행하지 않았다.
- [ ] 완성 답안 `.c` 파일을 README에 포함하지 않았다.
