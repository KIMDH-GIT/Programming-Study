# 20-2 실습: `typedef`
이론: [note](../../../notes/20-enum-typedef-union/20-2-typedef.md)

## 실습 목적
typedef alias와 tag의 역할을 구분한다.
## 작성할 파일
`typedef_enum.c`
## 해야 할 일
named enum을 정의하고 typedef alias를 붙여 두 표기로 object를 선언한다.
## 사용할 개념
typedef name, tag, ordinary identifier namespace.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic typedef_enum.c -o typedef_enum
```
## 실행 방법
```sh
./typedef_enum
```
## 예상 관찰 결과
두 표기가 같은 declared enum type을 나타낸다.
## 확인 포인트
typedef가 새 distinct type이나 object를 만들지 않는다.
## 추가 실습
- ★ scalar alias를 만든다.
- ★★ tag 없는 형태를 비교한다.
- ★★★ namespace 표를 작성한다.
## 완료 기준
tag와 alias를 말로 구분하고 warning 없이 실행한다.
