# 13-3 실습: 문자열의 `'\0'` 종료

이론: [note](../../../notes/13-characters-and-strings/13-3-null-termination.md)
## 실습 목적
null terminator를 기준으로 string length를 계산한다.
## 작성할 파일
`null_termination.c`
## 해야 할 일
`{'C','a','t','\\0'}` 배열을 만들고 `'\0'` 전까지 count한다.
## 사용할 개념
null character, terminator, char array, loop, `size_t`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic null_termination.c -o null_termination
```
## 실행 방법
```sh
./null_termination
```
## 예상 관찰 결과
`Cat`과 length 3이 출력된다.
## 확인 포인트
terminator를 length에 포함하지 않는가?
## 추가 실습
- ★ 빈 string - ★★ 중간 null - ★★★ valid string 판별
## 완료 기준
- [ ] 경고 없음 - [ ] length 3 - [ ] `'0'`과 `'\0'` 구분
