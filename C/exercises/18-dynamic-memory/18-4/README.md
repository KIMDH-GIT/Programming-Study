# 18-4 실습: allocation size overflow
이론: [note](../../../notes/18-dynamic-memory/18-4-allocation-size-overflow.md)

## 실습 목적
원소 수와 element size 곱셈을 allocation 전에 검사한다.
## 작성할 파일
`allocation_overflow.c`
## 해야 할 일
작은 `size_t count`에 `SIZE_MAX / sizeof *p` guard를 적용하고 allocation·사용·해제한다.
## 사용할 개념
`size_t`, `SIZE_MAX`, unsigned wrap, allocation failure.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic allocation_overflow.c -o allocation_overflow
```
## 실행 방법
```sh
./allocation_overflow
```
## 예상 관찰 결과
정상 count에서는 첫 element 값이 출력된다.
## 확인 포인트
overflow와 malloc failure를 별도로 검사한다.
## 추가 실습
- ★ 다른 element type
- ★★ guard 함수 없이 조건 설명
- ★★★ 사용자 입력 검증 단계 작성
## 완료 기준
곱셈 전 guard와 allocation 후 NULL 검사가 있다.
