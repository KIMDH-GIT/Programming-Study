# 11-10 실습: 배열 검색

이론: [note](../../../notes/11-arrays/11-10-array-search.md)
## 실습 목적
배열을 순서대로 비교해 첫 일치 index를 찾는다.
## 작성할 파일
`array_search.c`
## 해야 할 일
`{4, 7, 1, 7, 9}`에서 target 7의 첫 index를 찾고 결과를 출력한다.
## 사용할 개념
linear search, target, current index, sentinel, `break`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_search.c -o array_search
```
## 실행 방법
```sh
./array_search
```
## 예상 관찰 결과
첫 일치 위치인 index 1이 출력된다.
## 확인 포인트
not-found sentinel을 배열 접근에 사용하지 않는가?
## 추가 실습
- ★ 첫 element 검색 - ★★ 없는 값 - ★★★ 일치 개수
## 완료 기준
- [ ] 경고 없음 - [ ] index 1 - [ ] not-found 처리
