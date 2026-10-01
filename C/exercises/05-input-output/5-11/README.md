# 5-11 실습: `scanf` 반환값

이론: [5-11 note](../../../notes/05-input-output/5-11-scanf-return-value.md)

## 실습 목적
성공 대입 수로 입력 상태를 구별한다.

## 작성할 파일
`scanf_result.c`

## 해야 할 일

1. `int left = -1;`, `int right = -2;`, `int matched;`를 선언한다.
2. `%d %d`로 정수 둘을 읽고 반환값을 `matched`에 저장한다.
3. `matched`, `EOF`, `left`, `right`를 한 줄에 출력한다.
4. 정상 입력, 두 번째 변환의 matching failure, 첫 변환의 matching failure, 첫 대입 전 EOF를 각각 새 프로세스로 실행한다.
5. 각 입력 문자열과 반환값, 두 객체의 최종 상태를 표로 기록한다.

## 사용할 개념
대입 수, 일치 실패, `EOF`, 초기화.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror scanf_result.c -o scanf_result
```

## 실행 방법

정상 입력:

```sh
printf '%s\n' '12 34' | ./scanf_result
```

두 번째 변환의 matching failure:

```sh
printf '%s\n' '12 x' | ./scanf_result
```

첫 변환의 matching failure:

```sh
printf '%s\n' 'x 34' | ./scanf_result
```

첫 대입 전 EOF:

```sh
./scanf_result < /dev/null
```

## 예상 관찰 결과

| 입력 | `matched` | `left` | `right` |
|---|---:|---:|---:|
| `12 34` | 2 | 12 | 34 |
| `12 x` | 1 | 12 | -2 |
| `x 34` | 0 | -1 | -2 |
| 첫 대입 전 EOF | 출력한 `EOF`와 같음 | -1 | -2 |

부분 성공에서는 성공한 앞 대입만 유지된다. 첫 matching failure와 첫 대입 전 input failure는 모두 객체를 바꾸지 않지만 반환값은 각각 0과 `EOF`로 다르다.

## 확인 포인트

- 결과 객체를 초기화했는가?
- 반환값과 입력값을 구별했는가?
- 반환값과 실제로 바뀐 객체 수가 일치하는가?
- matching failure 0과 input failure `EOF`를 구별했는가?
- 실패를 일으킨 문자 `x`가 stream에 남을 수 있음을 설명했는가?

## 추가 실습

- ★ **기초:** 두 음수를 정상 입력하고 반환값 2를 확인한다.
- ★★ **응용:** 두 번째 변환만 실패시키는 다른 문자열을 설계하고 결과를 예측한다.
- ★★★ **도전:** 같은 실패 입력을 무조건 다시 읽으면 같은 문자에서 재실패할 수 있는 이유를 설명한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 네 입력 경로를 모두 기록했다.
- [ ] 정상 입력에서 `matched=2`, `left=12`, `right=34`를 확인했다.
- [ ] 부분 실패에서 `matched=1`, `left=12`, `right=-2`를 확인했다.
- [ ] 첫 matching failure에서 `matched=0`과 두 초기값 유지를 확인했다.
- [ ] EOF 경로에서 `matched == EOF`와 두 초기값 유지를 확인했다.
- [ ] 성공한 대입 수와 객체 상태를 연결해 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
