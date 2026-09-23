# 23-9 실습: 구조체 통째 저장의 padding·pointer·호환성 문제
이론: [note](../../../notes/23-file-io/23-9-raw-struct-storage-problems.md)
## 실습 목적
raw struct persistence 대신 logical fields를 명시적으로 encoding한다.
## 작성할 파일
`struct_storage.c`
## 해야 할 일
pointer member가 있는 student의 id·score·string contents를 text로 저장한다.
## 사용할 개념
padding, endianness, object representation, pointer lifetime, serialization.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic struct_storage.c -o struct_storage
```
## 실행 방법
```sh
./struct_storage
```
## 예상 관찰 결과
`fields encoded as text`가 출력된다.
## 확인 포인트
pointer value나 raw structure bytes를 portable file format이라고 부르지 않는다.
## 추가 실습
- ★ structure size를 관찰한다.
- ★★ field length contract를 작성한다.
- ★★★ explicit binary encoding을 설계한다.
## 완료 기준
padding·pointer·ABI 문제를 설명하고 fields만 안전하게 저장한다.
