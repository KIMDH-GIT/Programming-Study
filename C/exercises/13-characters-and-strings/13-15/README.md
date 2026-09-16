# 13-15 실습: 문자 종류 개수 분석

이론: [note](../../../notes/13-characters-and-strings/13-15-character-count-analysis.md)
## 실습 목적
valid ctype argument로 네 character categories를 센다.
## 작성할 파일
`character_count.c`
## 해야 할 일
string의 uppercase, lowercase, digit, whitespace counts를 출력한다.
## 사용할 개념
string traversal, `<ctype.h>`, `(unsigned char)`, `size_t`, locale.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic character_count.c -o character_count
```
## 실행 방법
```sh
./character_count
```
## 예상 관찰 결과
예제 input에서 upper 1, lower 1, digit 1, whitespace 2다.
## 확인 포인트
ctype 호출 전에 `(unsigned char)`로 변환하는가?
## 추가 실습
- ★ punctuation - ★★ newline/tab - ★★★ locale 조사
## 완료 기준
- [ ] 경고 없음 - [ ] counts 정확 - [ ] argument 범위 설명
