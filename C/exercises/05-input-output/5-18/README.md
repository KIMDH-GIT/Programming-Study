# 5-18 실습: Part 5 종합 복습

이론: [5-18 note](../../../notes/05-input-output/5-18-part-5-review.md)

## 실습 목적

여러 출력·입력 서식, `scanf` 반환값, 문자열 buffer 폭, 안전한 diagnostic 분석, 제한된 산술과 단위 출력을 하나의 재현 가능한 Part 5 self-check로 연결한다.

## 작성할 파일

- `part5_review.c`: 정상·부분 실패·EOF 입력을 관찰하고 고정 계산 결과를 출력하는 프로그램
- `format_contracts.md`: 출력 값형/입력 포인터형 대응표와 mismatch 분석표
- `run_observations.md`: 다섯 실행 사례의 입력, 반환값, 객체 상태, 출력 관찰 기록

## 해야 할 일

1. `format_contracts.md`에 다음 두 표를 작성한다.
   - `printf`: `%d`, `%u`, `%x`, `%f`, `%Lf`, `%c`, `%s`, `%p`, `%zu`와 요구 값형
   - `scanf`: `%d`, `%u`, `%x`, `%f`, `%lf`, `%Lf`, `%c`, `%15s`와 요구 저장 대상형
2. `part5_review.c`에 아래 객체를 선언하고 지정한 초기값을 사용한다.
   - `int signed_value = -1`
   - `unsigned int hex_value = 0U`
   - `double measured = -999.0`
   - `char marker = 'C'`
   - `char word[16] = "unchanged"`
3. `scanf` 한 번으로 signed 10진, unsigned 16진, `double`, 최대 15자의 단어를 이 순서로 읽고 반환값을 저장한다.
4. 반환값과 `EOF`를 함께 출력하고, 네 입력 객체를 각각 올바른 `printf` 서식으로 출력한다.
5. `marker`, `42U`의 10진·16진 표기를 한 `printf` 호출로 출력하고 그 호출의 반환 문자 수도 출력한다.
6. 고정값 25.0°C를 화씨로 변환하고 `77.00 F`가 보이도록 출력한다.
7. 고정값 7384초를 정수 몫과 나머지로 `2:03:04` 형식으로 출력한다.
8. `signed_value`의 주소를 `(void *)`와 `%p`로, 크기를 `%zu`로 출력한다.
9. 아래 다섯 실행 사례를 그대로 재현하고 `run_observations.md`에 반환값과 네 입력 객체의 최종 상태를 기록한다.
   - 정상 입력
   - 두 번째 변환에서 matching failure
   - 첫 변환에서 matching failure
   - 첫 변환 전 EOF
   - 16자 token의 `%15s` 경계
10. `format_contracts.md`에 다음 mismatch 네 종류를 “요구형/실제형/C17 분류/수정 방향”으로 분석한다. 코드를 실행하지 않는다.
    - 출력 `%d`와 `double`
    - 출력 `%Lf`와 `double`
    - 입력 `%d`와 `double *`
    - 입력 `%lf`와 `float *`

조건문, 반복문, 별도 함수는 필수로 사용하지 않는다. 초기값과 반환값을 함께 출력해 각 상태를 안전하게 관찰한다.

## 사용할 개념

`printf`, `printf` 반환값, 정수·부동소수점·문자·문자열·주소·크기 출력, `scanf`, 값 인자와 포인터 인자, assignment count, matching failure, `EOF`, `%15s`, format mismatch, 정수 `/`와 `%`, 실수 비율, 단위 출력.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror part5_review.c -o part5_review
```

## 실행 방법

정상 입력:

```sh
printf '7 2a 25.0 C17\n' | ./part5_review
```

두 번째 변환에서 matching failure:

```sh
printf '7 nope 25.0 C17\n' | ./part5_review
```

첫 변환에서 matching failure:

```sh
printf 'nope 2a 25.0 C17\n' | ./part5_review
```

첫 변환 전 EOF:

```sh
./part5_review < /dev/null
```

16자 token의 `%15s` 경계:

```sh
printf '7 2a 25.0 abcdefghijklmnop\n' | ./part5_review
```

## 예상 관찰 결과

### 정상 입력

- `matched=4`
- `signed=7`
- 16진 입력 `2a`는 값 42로 저장되어 `hex=0x2a`로 표시된다.
- `measured=25.00`
- `word=C17`

### 두 번째 변환의 matching failure

- `matched=1`
- 첫 객체만 `signed=7`로 바뀐다.
- `hex_value`, `measured`, `word`는 각각 초기값 `0`, `-999.00`, `unchanged`를 유지한다.
- 실패 문자인 `n`은 자동으로 유효한 16진 입력으로 바뀌지 않는다.

### 첫 변환의 matching failure

- `matched=0`
- 네 입력 객체가 모두 초기값을 유지한다.

### 첫 변환 전 EOF

- `matched`와 함께 출력한 `EOF` 값이 서로 같다.
- 네 입력 객체가 모두 초기값을 유지한다.
- `EOF`의 구체적인 음수값을 문서에 하드코딩하지 않는다.

### 16자 token

- `matched=4`
- `word`에는 앞의 15문자 `abcdefghijklmno`가 저장되고 null 문자로 끝난다.
- 마지막 문자 `p`는 이 `%15s` 변환이 소비하지 않은 입력으로 남는다.

### 고정 계산과 구현 관찰

- 온도 출력에 `77.00 F`가 보인다.
- 시간 출력에 `2:03:04`가 보인다.
- `42U`는 10진 `42`와 16진 `0x2a`로 표시된다.
- 주소의 문자 형태와 `int`의 C byte 크기는 구현 관찰값이므로 한 숫자로 고정하지 않는다.

## 확인 포인트

- `printf`에는 값, `scanf`에는 정확한 저장 포인터를 전달했는가?
- 입력 `%f`/`%lf`와 출력 `%f`의 규칙을 구별했는가?
- 각 실행에서 반환값과 실제로 바뀐 객체 수가 일치하는가?
- matching failure와 첫 변환 전 EOF를 구별했는가?
- `%15s`가 16칸 배열에 null 문자 공간을 남기는가?
- mismatch 네 사례를 실행 결과가 아니라 contract와 diagnostic 관점으로 분석했는가?
- 정수 나눗셈·나머지, 실수 비율, 단위와 출력 서식이 서로 맞는가?
- 주소와 `sizeof` 결과를 구현 전체에 고정된 값처럼 해석하지 않았는가?

## 추가 실습

- ★ **기초:** `format_contracts.md`의 각 행에 올바른 인자 예를 하나씩 추가한다.
- ★★ **응용:** 정상 입력의 `measured`를 0.0과 -40.0으로 바꾸고 값 저장과 고정 온도 계산을 구별해 기록한다.
- ★★★ **도전:** 5-12의 compile-only diagnostic 파일을 다시 검사하고, 네 mismatch의 GCC diagnostic과 C17 의미 분류를 review 표에 연결한다.

## 완료 기준

- [ ] `part5_review.c`가 strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 다섯 실행 사례를 모두 재현해 `run_observations.md`에 기록했다.
- [ ] 정상 입력의 반환값 4와 네 저장값이 예상과 일치한다.
- [ ] 부분 실패의 반환값 1과 첫 객체만 변경된 상태가 예상과 일치한다.
- [ ] 첫 matching failure의 반환값 0과 초기값 유지가 예상과 일치한다.
- [ ] EOF 실행에서 `matched == EOF`임을 출력으로 확인했다.
- [ ] 16자 token에서 앞 15문자만 저장되는 경계를 설명했다.
- [ ] `77.00 F`, `2:03:04`, 42의 10진·16진 표기를 확인했다.
- [ ] 출력 값형/입력 포인터형 대응표를 모두 완성했다.
- [ ] 네 mismatch를 실행하지 않고 요구형·실제형·C17 분류·수정 방향으로 분석했다.
- [ ] 주소 표현과 `sizeof(int)`를 구현 관찰로 기록했다.
- [ ] Part 6 이상의 파일이나 문법을 필수 해결책으로 사용하지 않았다.
- [ ] 완성 답안 소스를 README에 포함하지 않았다.

## 다음 Step

Step 6-1. implicit conversion
