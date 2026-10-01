# 5-2 실습: 정수 출력 서식

이론: [5-2 note](../../../notes/05-input-output/5-2-integer-output-formats.md)

## 실습 목적
signed 10진, unsigned 10진, unsigned 16진 출력을 구별한다.

## 작성할 파일
`integer_output.c`

## 해야 할 일

1. `int signed_value = -42;`와 `unsigned int unsigned_value = 42U;`를 선언한다.
2. `signed_value`를 `%d`로 출력한다.
3. `unsigned_value`를 `%u`와 `%x`로 각각 출력한다.
4. 16진 출력 앞의 `0x`는 서식 문자열의 일반 문자로 직접 작성한다.
5. 같은 `unsigned_value`를 `%08x`로 한 번 더 출력해 0 채움 결과를 비교한다.

## 사용할 개념
`%d`, `%u`, `%x`, 필드 폭, 0 채움.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror integer_output.c -o integer_output
```

## 실행 방법

```sh
./integer_output
```

## 예상 관찰 결과

```text
signed=-42
unsigned=42
hex=0x2a
hex padded=0x0000002a
```

`%u`와 `%x`는 같은 unsigned 값 42를 다른 진법으로 표시한다. `%08x`의 8은 최소 출력 폭이며 정수형의 bit 수가 아니다.

## 확인 포인트

- 실제 인자형과 서식이 맞는가?
- `0x`를 일반 문자로 썼는가?
- `%x`와 `%08x`의 값은 같고 표시 폭만 다른가?
- 16진 출력 문자로 객체의 byte 순서를 추측하지 않았는가?

## 추가 실습

- ★ **기초:** `255U`를 `%u`와 `%x`로 출력하고 `255`, `ff`를 확인한다.
- ★★ **응용:** `%8x`와 `%08x`를 비교해 공백 채움과 0 채움을 기록한다.
- ★★★ **도전:** 출력과 객체 표현을 구별한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 네 출력 줄이 예상 결과와 일치한다.
- [ ] `%d`, `%u`, `%x`, `%08x`의 요구형과 표시 차이를 설명했다.
- [ ] `0x`가 일반 문자임을 설명했다.
- [ ] 형식 불일치를 만들지 않았다.
- [ ] 답안 `.c`를 제공하지 않았다.
