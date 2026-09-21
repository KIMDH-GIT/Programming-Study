# 18-6 실습: 여러 element allocation
이론: [note](../../../notes/18-dynamic-memory/18-6-malloc-count-sizeof.md)

## 실습 목적
pointer+count로 allocated array를 관리한다.
## 작성할 파일
`dynamic_values.c`
## 해야 할 일
다섯 `int`를 `malloc(count * sizeof *values)`로 allocation해 1~5를 저장·출력·해제한다.
## 사용할 개념
element count, `sizeof *values`, bounded subscript.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic dynamic_values.c -o dynamic_values
```
## 실행 방법
```sh
./dynamic_values
```
## 예상 관찰 결과
1 2 3 4 5가 출력된다.
## 확인 포인트
base pointer와 count를 유지한다.
## 추가 실습
- ★ double 배열
- ★★ pointer arithmetic 순회
- ★★★ separate iterator pointer
## 완료 기준
overflow·failure·bounds·free 규칙을 지킨다.
