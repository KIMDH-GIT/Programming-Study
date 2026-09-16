# 13-6 실습: 문자 배열 매개변수 문법 예고

이론: [note](../../../notes/13-characters-and-strings/13-6-char-array-parameter-preview.md)
## 실습 목적
char array parameter notation을 제한된 범위에서 사용한다.
## 작성할 파일
`char_array_parameter.c`
## 해야 할 일
`void print_text(char text[])`를 정의하고 valid string array를 전달한다.
## 사용할 개념
array parameter notation, adjustment preview, valid string, function call.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic char_array_parameter.c -o char_array_parameter
```
## 실행 방법
```sh
./char_array_parameter
```
## 예상 관찰 결과
전달한 message가 한 줄 출력된다.
## 확인 포인트
parameter 안의 `sizeof`로 caller array count를 구하지 않는가?
## 추가 실습
- ★ 첫 character - ★★ 두 호출 - ★★★ notation 비교표
## 완료 기준
- [ ] 경고 없음 - [ ] message 출력 - [ ] adjustment 제한 설명
