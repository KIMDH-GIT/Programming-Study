# 20-9 실습: `unsigned char`로 byte representation 관찰
이론: [note](../../../notes/20-enum-typedef-union/20-9-observe-byte-representation.md)

## 실습 목적
object의 bytes를 허용된 character-type access로 관찰한다.
## 작성할 파일
`byte_representation.c`
## 해야 할 일
unsigned integer의 `sizeof`만큼 bytes를 16진수로 출력한다.
## 사용할 개념
object representation, `unsigned char *`, `%02X`, `sizeof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic byte_representation.c -o byte_representation
```
## 실행 방법
```sh
./byte_representation
```
## 예상 관찰 결과
현재 implementation의 byte sequence가 출력된다.
## 확인 포인트
특정 순서·길이를 표준 고정값으로 가정하지 않는다.
## 추가 실습
- ★ 다른 값을 관찰한다.
- ★★ structure를 관찰한다.
- ★★★ semantic/representation 비교표를 만든다.
## 완료 기준
관찰값을 implementation 결과로 정확히 기록한다.
