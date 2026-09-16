# 11-2 실습: index로 원소 읽기·쓰기

이론: [note](../../../notes/11-arrays/11-2-index-read-write.md)
## 실습 목적
특정 index의 element를 읽고 수정한다.
## 작성할 파일
`index_read_write.c`
## 해야 할 일
5개 배열에서 index 1과 3을 출력하고 각각 200, 400으로 바꾼 뒤 다시 출력한다.
## 사용할 개념
subscript, index, element, assignment, 유효 범위.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic index_read_write.c -o index_read_write
```
## 실행 방법
```sh
./index_read_write
```
## 예상 관찰 결과
수정 전 값과 200, 400으로 바뀐 값이 출력된다.
## 확인 포인트
배열 전체가 아니라 지정한 두 element만 바뀌는가?
## 추가 실습
- ★ index 0 수정 - ★★ element 계산 - ★★★ 경계 판별
## 완료 기준
- [ ] 경고 없음 - [ ] 수정 결과 정확 - [ ] index 범위 준수
