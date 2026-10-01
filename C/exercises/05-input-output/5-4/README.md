# 5-4 실습: 문자와 문자열 출력

이론: [5-4 note](../../../notes/05-input-output/5-4-character-string-output.md)

## 실습 목적
`%c`, `%s`, 문자열 정밀도를 구별한다.

## 작성할 파일
`character_string_output.c`

## 해야 할 일

1. `char letter = 'C';`와 문자열 리터럴 `"language"`를 사용한다.
2. `letter`를 `%c`로 출력한다.
3. `"language"` 전체를 `%s`로 출력한다.
4. 같은 문자열의 앞 네 문자만 `%.4s`로 출력한다.
5. 정밀도 4가 원본 문자열을 수정하는 것이 아니라 최대 출력 문자 수를 제한한다는 설명을 기록한다.

## 사용할 개념
문자 상수, 문자열 리터럴, `%c`, `%s`, 정밀도.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror character_string_output.c -o character_string_output
```

## 실행 방법

```sh
./character_string_output
```

## 예상 관찰 결과

```text
letter=C
word=language
prefix=lang
```

`%.4s`는 `language`의 앞 네 문자만 표시한다. 문자열 리터럴의 내용과 null 종료는 바뀌지 않는다.

## 확인 포인트

- 인자 종류가 서식과 맞는가?
- `%c`가 문자 하나, `%s`가 null 종료 문자열을 요구한다는 차이를 설명했는가?
- `prefix=lang`가 정확히 네 문자인가?
- 출력 정밀도와 입력 buffer 폭을 혼동하지 않았는가?

## 추가 실습

- ★ **기초:** `letter`를 다른 문자로 바꾸고 첫 줄만 달라지는지 확인한다.
- ★★ **응용:** `%.2s`와 `%.5s`를 비교해 예상 prefix를 먼저 기록한다.
- ★★★ **도전:** 종료 없는 문자열의 위험을 설명한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 세 출력 줄이 예상 결과와 정확히 일치한다.
- [ ] `%c`와 `%s`를 구별했다.
- [ ] `%.4s`가 원본을 수정하지 않는 이유를 설명했다.
- [ ] null 종료가 `%s`의 유효한 읽기 경계임을 설명했다.
- [ ] 답안 `.c`를 제공하지 않았다.
