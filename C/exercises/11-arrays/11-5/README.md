# 11-5 실습: `sizeof(array)`와 원소 수

이론: [note](../../../notes/11-arrays/11-5-sizeof-element-count.md)
## 실습 목적
배열 byte 수와 element count를 구분한다.
## 작성할 파일
`array_size_count.c`
## 해야 할 일
5개 `int` 배열의 전체 크기, element 하나 크기, 계산한 count를 `%zu`로 출력한다.
## 사용할 개념
`sizeof`, `size_t`, `%zu`, element count, C byte.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_size_count.c -o array_size_count
```
## 실행 방법
```sh
./array_size_count
```
## 예상 관찰 결과
element count는 5이며 byte 수는 구현의 `sizeof(int)`에 따라 정해진다.
## 확인 포인트
전체 byte 수를 count라고 부르지 않는가?
## 추가 실습
- ★ `double` 배열 - ★★ count로 순회 - ★★★ 크기 비교표
## 완료 기준
- [ ] 경고 없음 - [ ] count 5 - [ ] `%zu` 사용
