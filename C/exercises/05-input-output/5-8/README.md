# 5-8 실습: 정수 입력 서식

이론: [5-8 note](../../../notes/05-input-output/5-8-integer-input-formats.md)

## 실습 목적
세 정수 입력 서식과 저장 대상형을 맞춘다.

## 작성할 파일
`integer_input.c`

## 해야 할 일

1. 다음 초기값으로 객체를 선언한다.
   - `int decimal = -1`
   - `unsigned int unsigned_decimal = 99U`
   - `unsigned int hexadecimal = 77U`
2. `%d %u %x`와 세 객체의 주소를 사용해 한 줄에서 값을 읽는다.
3. `matched`와 세 객체를 `%d`, `%u`, `%x`로 출력한다.
4. 정상 입력, 두 번째 변환의 matching failure, 첫 변환의 matching failure를 각각 새 프로세스로 실행한다.
5. 각 실행에서 성공한 대입 수와 바뀐 객체·초기값을 유지한 객체를 표로 기록한다.

## 사용할 개념
`%d`, `%u`, `%x`, `int *`, `unsigned int *`, 객체 주소, assignment count, matching failure.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror integer_input.c -o integer_input
```

## 실행 방법

정상 입력:

```sh
printf '%s\n' '-12 12 2a' | ./integer_input
```

두 번째 변환의 matching failure:

```sh
printf '%s\n' '-12 nope 2a' | ./integer_input
```

첫 변환의 matching failure:

```sh
printf '%s\n' 'nope 12 2a' | ./integer_input
```

## 예상 관찰 결과

| 사례 | `matched` | `decimal` | `unsigned_decimal` | `hexadecimal`의 `%x` 출력 |
|---|---:|---:|---:|---|
| `-12 12 2a` | 3 | -12 | 12 | `2a` |
| `-12 nope 2a` | 1 | -12 | 99 | `4d` |
| `nope 12 2a` | 0 | -1 | 99 | `4d` |

초기값 77을 `%x`로 출력하면 `4d`다. 중간 변환이 실패하면 그 뒤 변환은 진행되지 않는다.

## 확인 포인트

- signed/unsigned 주소형이 맞는가?
- 모든 객체를 초기화했는가?
- 반환값과 실제로 변경된 객체 수가 일치하는가?
- `%x` 입력 `2a`가 수학적 값 42로 저장된다는 점을 설명할 수 있는가?
- 실패 뒤 `99`와 `4d`가 새 입력값이 아니라 초기값의 두 표기임을 구별했는가?

## 추가 실습

- ★ **기초:** `0 0 0`을 입력해 세 형에서 0을 확인한다.
- ★★ **응용:** 세 번째 변환만 실패시키고 반환값과 앞 두 객체의 상태를 예측한다.
- ★★★ **도전:** 범위 밖 입력 위험을 정리한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 세 실행 사례를 모두 기록했다.
- [ ] 정상 입력에서 `matched=3`, `-12`, `12`, `2a`를 확인했다.
- [ ] 두 번째 변환 실패에서 `matched=1`과 뒤 두 초기값 유지를 확인했다.
- [ ] 첫 변환 실패에서 `matched=0`과 세 초기값 유지를 확인했다.
- [ ] `%d`는 `int *`, `%u`·`%x`는 `unsigned int *`를 요구함을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
