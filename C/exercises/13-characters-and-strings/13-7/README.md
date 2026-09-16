# 13-7 실습: `my_strlen`

이론: [note](../../../notes/13-characters-and-strings/13-7-my-strlen.md)
## 실습 목적
null terminator 전까지 직접 string length를 센다.
## 작성할 파일
`my_strlen.c`
## 해야 할 일
`size_t my_strlen(char text[])`를 작성해 세 valid strings의 길이를 출력한다.
## 사용할 개념
null terminator, array parameter, `size_t`, traversal.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic my_strlen.c -o my_strlen
```
## 실행 방법
```sh
./my_strlen
```
## 예상 관찰 결과
empty 0, `"A"` 1, `"hello"` 5가 확인된다.
## 확인 포인트
terminator를 세지 않고 valid strings만 전달하는가?
## 추가 실습
- ★ empty - ★★ spaces 포함 - ★★★ 추적표
## 완료 기준
- [ ] 경고 없음 - [ ] 0/1/5 정확 - [ ] `size_t` 사용
