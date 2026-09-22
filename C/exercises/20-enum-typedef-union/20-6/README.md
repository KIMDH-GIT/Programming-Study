# 20-6 실습: tag와 union을 함께 쓰기
이론: [note](../../../notes/20-enum-typedef-union/20-6-tagged-union.md)

## 실습 목적
tagged union invariant를 유지하며 variant를 출력한다.
## 작성할 파일
`tagged_value.c`
## 해야 할 일
int/double variants를 만들고 tag 검사 뒤 matching member만 읽는다.
## 사용할 개념
enum tag, union, structure, switch, const pointer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic tagged_value.c -o tagged_value
```
## 실행 방법
```sh
./tagged_value
```
## 예상 관찰 결과
각 variant가 올바른 format으로 출력된다.
## 확인 포인트
tag와 member를 동시에 일관되게 설정한다.
## 추가 실습
- ★ constructor를 만든다.
- ★★ invalid tag를 분석한다.
- ★★★ semantic equality를 구현한다.
## 완료 기준
모든 read 전에 matching tag를 확인한다.
