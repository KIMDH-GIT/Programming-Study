# 21-12 실습: bit field 추출·갱신
이론: [note](../../../notes/21-bitwise-operators/21-12-bit-field-extract-update.md)
## 실습 목적
연속 field를 추출하고 안전하게 갱신한다.
## 작성할 파일
`field_update.c`
## 해야 할 일
3-bit field를 읽고 새 값으로 교체한다.
## 사용할 개념
mask, shifts, clear, insert.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic field_update.c -o field_update
```
## 실행 방법
```sh
./field_update
```
## 예상 관찰 결과
기존 field와 갱신된 전체 값이 출력된다.
## 확인 포인트
인접 bits를 보존한다.
## 추가 실습
- ★ 값을 바꾼다.
- ★★ range를 검사한다.
- ★★★ helper를 만든다.
## 완료 기준
field 외 bits를 보존해 갱신한다.
