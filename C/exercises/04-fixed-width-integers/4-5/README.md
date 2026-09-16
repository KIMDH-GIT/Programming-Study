# 4-5 실습: 한계·상수·출력 매크로

이론: [4-5. 범위·상수 매크로와 `<inttypes.h>` 출력](../../../notes/04-fixed-width-integers/4-5-limits-constants-inttypes-output.md)

## 실습 목적

32-bit exact-width 값에 맞는 한계, 상수, 출력 매크로를 한 프로그램에서 올바르게 조합한다.

## 작성할 파일

`portable_output.c`

## 해야 할 일

1. `<inttypes.h>`와 `<stdio.h>`를 포함한다.
2. `int32_t` 음수를 `-INT32_C(...)`로 초기화한다.
3. `uint32_t` 큰 양수를 `UINT32_C(...)`로 초기화한다.
4. `PRId32`, `PRIu32`, `PRIx32`로 두 값을 출력한다.
5. `INT32_MIN`, `INT32_MAX`, `UINT32_MAX`를 대응 서식으로 출력한다.
6. `<stdint.h>`와 `<inttypes.h>`의 역할 차이를 두 문장으로 기록한다.

## 사용할 개념

exact-width 한계 매크로, integer constant macro, `PRI` 출력 매크로, 인접 문자열 리터럴 결합, 가변 인수와 서식 일치.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic portable_output.c -o portable_output
```

## 실행 방법

```sh
./portable_output
```

## 예상 관찰 결과

signed 값은 음의 10진수, unsigned 값은 10진수와 16진수로 표시된다. 32-bit 한계값이 잘리지 않고 출력된다.

## 확인 포인트

- 음수에서 단항 `-`와 양의 `INT32_C` 호출을 구별했는가?
- `PRI` 매크로 앞에 `"%"`가 있는가?
- signed, unsigned, 16진 출력 매크로가 값의 형과 목적에 맞는가?
- `%d`, `%u`, `%ld`를 바탕형 추측으로 고정하지 않았는가?
- `INT32_C`의 결과형 설명에 `int_least32_t`의 promoted type을 반영했는가?

## 추가 실습

- ★ **기초:** 16-bit 대응 한계 매크로 이름을 적는다.
- ★★ **응용:** 같은 `uint32_t` 값을 10진과 16진으로 비교한다.
- ★★★ **도전:** `gcc -E` 결과에서 결합 전 매크로 전개를 찾아 구현 선택과 표준 계약을 구별한다.

## 완료 기준

- [ ] C17 경고 옵션으로 진단 없이 컴파일된다.
- [ ] 실행 상태 0이다.
- [ ] 세 `PRI` 매크로가 올바르게 사용되었다.
- [ ] 세 32-bit 한계값을 출력했다.
- [ ] 상수 매크로의 역할을 새 C 문법으로 오해하지 않았다.
- [ ] 두 헤더의 관계를 설명했다.
- [ ] 완성 답안 `.c` 파일을 만들지 않았다.
