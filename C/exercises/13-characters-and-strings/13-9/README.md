# 13-9 실습: `my_strcmp`

이론: [note](../../../notes/13-characters-and-strings/13-9-my-strcmp.md)
## 실습 목적
두 valid strings의 first difference와 result sign을 확인한다.
## 작성할 파일
`my_strcmp.c`
## 해야 할 일
equal, before, after pairs를 비교하는 `my_strcmp`를 작성한다.
## 사용할 개념
parallel traversal, null terminator, `unsigned char`, comparison sign.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic my_strcmp.c -o my_strcmp
```
## 실행 방법
```sh
./my_strcmp
```
## 예상 관찰 결과
관계에 따라 0, 음수, 양수가 나온다.
## 확인 포인트
result magnitude가 아니라 sign으로 관계를 판단하는가?
## 추가 실습
- ★ equal - ★★ prefix - ★★★ index 추적
## 완료 기준
- [ ] 경고 없음 - [ ] 세 관계 정확 - [ ] arrays에 `==` 사용 안 함
