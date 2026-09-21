# 18-11 실습: failure-safe `realloc`
이론: [note](../../../notes/18-dynamic-memory/18-11-realloc-and-temporary-pointer.md)

## 실습 목적
temporary pointer로 allocation을 안전하게 확장한다.
## 작성할 파일
`realloc_values.c`
## 해야 할 일
2-element array를 overflow가 검사된 양수 크기의 3 elements로 확장하고 success 뒤 새 element를 초기화한다. failure면 original을 free한다.
## 사용할 개념
`realloc`, temporary pointer, content preservation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic realloc_values.c -o realloc_values
```
## 실행 방법
```sh
./realloc_values
```
## 예상 관찰 결과
기존 두 값과 새 값이 출력된다.
## 확인 포인트
zero-size 요청·직접 대입·확장 영역 uninitialized read가 없다.
## 추가 실습
- ★ flow diagram
- ★★ shrink case
- ★★★ new size overflow guard
## 완료 기준
temporary pointer pattern과 양쪽 cleanup 경로가 정확하다.
