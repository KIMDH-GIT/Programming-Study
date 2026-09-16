# 12-4 실습: row-major 메모리 배치

이론: [note](../../../notes/12-two-dimensional-arrays/12-4-row-major-layout.md)
## 실습 목적
array-of-arrays의 row-major element 순서를 추적한다.
## 작성할 파일
`row_major.c`
## 해야 할 일
2x3 배열의 row, column, value를 row-major 순서로 출력한다.
## 사용할 개념
row-major, array of arrays, contiguous elements, nested traversal.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic row_major.c -o row_major
```
## 실행 방법
```sh
./row_major
```
## 예상 관찰 결과
`(0,0)`부터 `(1,2)`까지 row 단위 순서로 출력된다.
## 확인 포인트
row-major를 CPU 보장이 아니라 C 배열 배치로 설명하는가?
## 추가 실습
- ★ 3x2 순서 - ★★ column 우선 출력 비교 - ★★★ 표준/cache 표
## 완료 기준
- [ ] 경고 없음 - [ ] 여섯 위치 순서 정확 - [ ] 구현 관점 구분
