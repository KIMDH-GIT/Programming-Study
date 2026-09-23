# 23-8 실습: `fread`와 `fwrite`
이론: [note](../../../notes/23-file-io/23-8-fread-and-fwrite.md)
## 실습 목적
block I/O의 element size, count, return value를 구분한다.
## 작성할 파일
`binary_block_io.c`
## 해야 할 일
세 integers를 write/read하고 returned element counts를 검사한다.
## 사용할 개념
`fread`, `fwrite`, element count, short read, partial write.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic binary_block_io.c -o binary_block_io
```
## 실행 방법
```sh
./binary_block_io
```
## 예상 관찰 결과
`10 20 30`이 출력된다.
## 확인 포인트
return을 항상 byte count로 설명하지 않고 binary buffer를 string으로 출력하지 않는다.
## 추가 실습
- ★ 두 elements만 읽는다.
- ★★ EOF short read를 관찰한다.
- ★★★ fixed byte encoding과 비교한다.
## 완료 기준
requested와 returned element counts를 비교해 warning 없이 round trip한다.
