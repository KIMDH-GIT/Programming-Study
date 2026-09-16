# 4-3 실습: unsigned exact-width 정수형

이론: [4-3. `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`](../../../notes/04-fixed-width-integers/4-3-unsigned-exact-width-integers.md)

## 실습 목적

네 unsigned exact-width 형의 값과 크기를 관찰하고, unsigned 범위와 외부 형식의 폭 계약을 연결한다.

## 작성할 파일

`unsigned_widths.c`

## 해야 할 일

1. 필요한 표준 헤더 세 개를 포함한다.
2. 네 `uintN_t` 변수를 대응하는 `UINTN_C` 상수로 초기화한다.
3. 각 값을 `unsigned long long`으로 변환해 `%llu`로 출력한다.
4. 각 객체의 `sizeof`와 `CHAR_BIT`를 출력한다.
5. 각 형의 수학적 범위를 기록한다.
6. 정확한 폭이 byte 순서를 보장하지 않는다는 문장을 적는다.

## 사용할 개념

`uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`, `UINTN_C`, unsigned 범위, `sizeof`, `CHAR_BIT`.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_widths.c -o unsigned_widths
```

## 실행 방법

```sh
./unsigned_widths
```

## 예상 관찰 결과

네 양수가 그대로 출력된다. 흔한 8-bit-byte 구현에서는 크기 1, 2, 4, 8이 보이지만 단위는 모두 C byte다.

## 확인 포인트

- 상수 매크로의 signed/unsigned 계열을 혼동하지 않았는가?
- 출력 변환 뒤 형과 서식이 일치하는가?
- `uint8_t`를 모든 구현의 의무형 또는 보편적인 byte형이라고 쓰지 않았는가?
- unsigned 순환과 업무상 올바름을 구별했는가?
- exact-width와 endianness를 구별했는가?

## 추가 실습

- ★ **기초:** 네 형의 최댓값을 2의 거듭제곱 식으로 쓴다.
- ★★ **응용:** `UINT8_MAX`를 저장한 변수와 그 다음 저장값을 관찰하고 중간 승격을 설명한다.
- ★★★ **도전:** 통신 필드에 필요한 계약을 폭과 byte 순서 두 항목으로 나누어 쓴다.

## 완료 기준

- [ ] 네 typedef가 있는 환경에서 C17 경고 없이 컴파일된다.
- [ ] 실행 상태 0과 예상 값을 확인했다.
- [ ] `sizeof`의 C byte 단위를 기록했다.
- [ ] 네 수학적 범위를 올바르게 적었다.
- [ ] 정수 승격을 모르는 채 중간 연산 폭을 단정하지 않았다.
- [ ] 외부 byte 순서가 별도 계약임을 설명했다.
- [ ] 완성 답안 소스를 만들지 않았다.
