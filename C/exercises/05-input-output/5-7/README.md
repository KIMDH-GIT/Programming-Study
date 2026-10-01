# 5-7 실습: `scanf`와 변수 주소

이론: [5-7 note](../../../notes/05-input-output/5-7-scanf-and-addresses.md)

## 실습 목적
입력 저장과 변수 주소, 반환값을 연결한다.

## 작성할 파일
`scanf_address.c`

## 해야 할 일

1. `int value = -99;`와 `int matched;`를 선언한다.
2. `%d`와 `&value`를 사용해 정수 하나를 읽고 반환값을 `matched`에 저장한다.
3. `matched`, `EOF`, `value`를 한 줄에 출력한다.
4. 정상 정수 `42`, 일치하지 않는 문자 `x`, 첫 변환 전 EOF의 세 사례를 각각 새 프로세스로 실행한다.
5. 각 실행에서 반환값과 `value`의 최종 상태를 표로 기록한다.

## 사용할 개념
`scanf`, `&`, 저장 객체, assignment count, matching failure, `EOF`, 초기값.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror scanf_address.c -o scanf_address
```

## 실행 방법

정상 입력:

```sh
printf '42\n' | ./scanf_address
```

matching failure:

```sh
printf 'x\n' | ./scanf_address
```

첫 변환 전 EOF:

```sh
./scanf_address < /dev/null
```

## 예상 관찰 결과

| 사례 | `matched` | `value` |
|---|---:|---:|
| `42` | 1 | 42 |
| `x` | 0 | -99 |
| 첫 변환 전 EOF | 출력한 `EOF`와 같음 | -99 |

matching failure와 input failure에서는 대입이 성공하지 않으므로 초기값 `-99`가 유지된다. `EOF`의 구체적인 음수값은 구현 헤더가 제공하므로 하드코딩하지 않는다.

## 확인 포인트

- 저장 인자에 `&`가 있는가?
- 값을 먼저 초기화했는가?
- 정상 입력과 matching failure에서 반환값이 각각 1과 0인가?
- EOF 실행에서 `matched`와 함께 출력한 `EOF`가 같은가?
- 실패 경로의 `value=-99`를 “새 입력값”으로 오해하지 않았는가?

## 추가 실습

- ★ **기초:** `-12`를 입력해 signed 정수 저장을 확인한다.
- ★★ **응용:** 공백 뒤의 정수도 `%d`가 읽는지 관찰한다.
- ★★★ **도전:** matching failure 문자가 stream에 남을 수 있는 이유와 무조건 재시도의 문제를 설명한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 정상·matching failure·EOF 세 실행을 모두 기록했다.
- [ ] 정상 입력에서 `matched=1`, `value=42`를 확인했다.
- [ ] 문자 입력에서 `matched=0`, `value=-99`를 확인했다.
- [ ] EOF 입력에서 `matched == EOF`, `value=-99`를 확인했다.
- [ ] 값 인자와 저장 주소의 차이를 설명했다.
- [ ] 반환값이 입력값 자체가 아니라 성공한 대입 수임을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
