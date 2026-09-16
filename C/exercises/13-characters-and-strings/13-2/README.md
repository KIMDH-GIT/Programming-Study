# 13-2 실습: 문자열 `"A"`와 문자 배열

이론: [note](../../../notes/13-characters-and-strings/13-2-string-literal-char-array.md)
## 실습 목적
string 내용 길이와 char array element count를 구분한다.
## 작성할 파일
`string_literal_array.c`
## 해야 할 일
`char word[] = "Cat";`을 출력하고 `sizeof(word)`를 확인한다.
## 사용할 개념
string literal, char array, null terminator, `sizeof`, `%s`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic string_literal_array.c -o string_literal_array
```
## 실행 방법
```sh
./string_literal_array
```
## 예상 관찰 결과
`Cat`과 element count 4가 출력된다.
## 확인 포인트
보이는 문자 3개와 array elements 4개를 구분하는가?
## 추가 실습
- ★ `"A"` 크기 - ★★ explicit initializer - ★★★ 길이/count 표
## 완료 기준
- [ ] 경고 없음 - [ ] `Cat`과 4 출력 - [ ] terminator 설명
