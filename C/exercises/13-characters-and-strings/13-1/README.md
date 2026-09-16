# 13-1 실습: `char`와 문자 `'A'`

이론: [note](../../../notes/13-characters-and-strings/13-1-char-character-constant.md)
## 실습 목적
문자 표현과 저장된 정수값을 구분한다.
## 작성할 파일
`char_value.c`
## 해야 할 일
`char letter = 'A';`를 `%c`와 `(int)` 변환 후 `%d`로 출력한다.
## 사용할 개념
`char`, ordinary character constant, integer conversion, `%c`, `%d`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic char_value.c -o char_value
```
## 실행 방법
```sh
./char_value
```
## 예상 관찰 결과
`A`와 실행 환경의 대응 정수값이 출력된다.
## 확인 포인트
관찰한 값을 모든 C 구현의 ASCII 보장이라고 설명하지 않는가?
## 추가 실습
- ★ 숫자 문자 - ★★ `sizeof` 비교 - ★★★ 표준/ASCII 표
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] 두 출력 관점 구분 - [ ] `'A'` type 설명
