# 18-16 실습: dynamic array bounds
이론: [note](../../../notes/18-dynamic-memory/18-16-dynamic-array-out-of-bounds.md)

## 실습 목적
allocated array를 count 범위 안에서만 접근한다.
## 작성할 파일
`dynamic_bounds.c`
## 해야 할 일
네 elements를 채우고 첫·마지막 값만 출력한다. 모든 loops는 `i < count`를 사용한다.
## 사용할 개념
subscript bounds, one-past pointer, pointer+count.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic dynamic_bounds.c -o dynamic_bounds
```
## 실행 방법
```sh
./dynamic_bounds
```
## 예상 관찰 결과
첫 값과 마지막 값이 출력된다.
## 확인 포인트
`values[count]` access가 없다.
## 추가 실습
- ★ bounds table
- ★★ pointer loop
- ★★★ realloc shrink bounds
## 완료 기준
모든 access가 allocated element range 안에 있다.
