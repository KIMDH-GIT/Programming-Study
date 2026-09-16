# 4-4 실습: 정수형 계열과 제공 조건

이론: [4-4. 정확한 폭의 타입이 제공되는 조건](../../../notes/04-fixed-width-integers/4-4-exact-width-availability.md)

## 실습 목적

least-width와 fast-width 형의 실제 선택을 관찰하고 exact-width의 선택 제공 조건과 비교한다.

## 작성할 파일

`width_families.c`

## 해야 할 일

1. `<limits.h>`, `<stdint.h>`, `<stdio.h>`를 포함한다.
2. `int_least16_t`와 `int_fast16_t` 변수를 같은 값으로 초기화한다.
3. 두 값을 `long long`으로 변환해 출력한다.
4. 각 형의 `sizeof`와 `sizeof * CHAR_BIT`를 출력한다.
5. exact, least, fast, pointer-conversion capable 계열의 계약을 표로 정리한다.
6. `intptr_t`와 `uintptr_t`는 이름과 목적만 기록하고 포인터 변환 코드는 쓰지 않는다.

## 사용할 개념

exact-width 선택 제공, minimum-width, fastest minimum-width, C byte, 저장 bit 수, 선택적 포인터 변환 정수형.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic width_families.c -o width_families
```

## 실행 방법

```sh
./width_families
```

## 예상 관찰 결과

두 값은 30000이다. 두 형의 C byte 크기와 저장 bit 수는 같을 수도 다를 수도 있으며, 어느 쪽도 이름만으로 정확히 16 bit라고 단정할 수 없다.

## 확인 포인트

- `least`를 “최소 요구 범위를 만족하는 가장 작은 폭”으로 설명했는가?
- `fast`를 “구현이 빠르다고 선택한 최소 범위형”으로 설명했는가?
- 저장 bit 수와 값 bit 수를 자동으로 같게 두지 않았는가?
- exact-width 선택 제공 조건을 기록했는가?
- 포인터 선행 학습 범위를 넘지 않았는가?

## 추가 실습

- ★ **기초:** 네 계열의 목적을 한 줄씩 쓴다.
- ★★ **응용:** 8·32에 대한 least/fast 형의 크기도 비교한다.
- ★★★ **도전:** 정확한 16-bit 형이 없는 구현에서도 최소 16-bit 계산이 가능한 이유를 설명한다.

## 완료 기준

- [ ] C17 경고 옵션으로 진단 없이 컴파일된다.
- [ ] 실행 상태 0이다.
- [ ] least와 fast의 값·크기를 기록했다.
- [ ] `int_least16_t`를 정확히 16 bit라고 쓰지 않았다.
- [ ] `int_fast16_t`를 정확히 16 bit라고 쓰지 않았다.
- [ ] `intptr_t`, `uintptr_t`의 선택 제공을 설명했다.
- [ ] 완성 답안 소스를 README에 포함하지 않았다.
