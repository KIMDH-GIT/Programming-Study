# 18-5 실습: `void *`와 `malloc`
이론: [note](../../../notes/18-dynamic-memory/18-5-void-pointer-and-malloc.md)

## 실습 목적
C17의 cast 없는 malloc 기본 pattern을 사용한다.
## 작성할 파일
`malloc_value.c`
## 해야 할 일
`<stdlib.h>`를 include하고 한 `int`를 allocation한다. NULL 검사 뒤 값을 저장·출력·해제한다.
## 사용할 개념
`void *`, implicit object-pointer conversion, uninitialized storage.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic malloc_value.c -o malloc_value
```
## 실행 방법
```sh
./malloc_value
```
## 예상 관찰 결과
저장한 값이 출력된다.
## 확인 포인트
malloc cast와 초기화 전 read가 없다.
## 추가 실습
- ★ double allocation
- ★★ `sizeof p` 비교
- ★★★ alignment contract 조사
## 완료 기준
`malloc(sizeof *p)` pattern과 failure 검사·free가 있다.
