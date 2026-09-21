# 18-9 실습: `calloc` all-bits-zero
이론: [note](../../../notes/18-dynamic-memory/18-9-calloc-all-bits-zero.md)

## 실습 목적
all-bits-zero storage를 안전한 type으로 관찰한다.
## 작성할 파일
`calloc_bytes.c`
## 해야 할 일
`unsigned char` 네 개를 calloc하고 모든 값이 0인지 출력한 뒤 해제한다.
## 사용할 개념
`calloc`, object representation, all-bits-zero.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic calloc_bytes.c -o calloc_bytes
```
## 실행 방법
```sh
./calloc_bytes
```
## 예상 관찰 결과
네 개의 0이 출력된다.
## 확인 포인트
모든 type의 semantic zero라고 일반화하지 않는다.
## 추가 실습
- ★ 일부 bytes 변경
- ★★ malloc loop initialization 비교
- ★★★ pointer representation 조사
## 완료 기준
failure 검사·bounds·free가 있고 caveat를 설명한다.
