# 13-4 실습: 문자열 리터럴과 수정 가능한 배열

이론: [note](../../../notes/13-characters-and-strings/13-4-literal-modifiable-array.md)
## 실습 목적
literal로 초기화한 char array element를 안전하게 수정한다.
## 작성할 파일
`modifiable_char_array.c`
## 해야 할 일
`char text[] = "hello";`의 첫 element를 `H`로 바꾸고 출력한다.
## 사용할 개념
string literal initialization, modifiable array, index, null terminator.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic modifiable_char_array.c -o modifiable_char_array
```
## 실행 방법
```sh
./modifiable_char_array
```
## 예상 관찰 결과
`Hello`가 출력된다.
## 확인 포인트
literal 자체가 아니라 array element를 수정하는가?
## 추가 실습
- ★ 마지막 문자 - ★★ 두 위치 - ★★★ 차이 표
## 완료 기준
- [ ] 경고 없음 - [ ] `Hello` 출력 - [ ] terminator 유지
