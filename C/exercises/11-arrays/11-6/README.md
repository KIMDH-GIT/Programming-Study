# 11-6 실습: 배열 범위 밖 접근과 Undefined Behavior

이론: [note](../../../notes/11-arrays/11-6-out-of-bounds-ub.md)
## 실습 목적
접근 전에 index 범위를 검사한다.
## 작성할 파일
`array_bounds.c`
## 해야 할 일
index 4와 5를 각각 검사하되 유효한 경우에만 배열 element를 읽는다.
## 사용할 개념
valid index, short-circuit, bounds check, Undefined Behavior.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_bounds.c -o array_bounds
```
## 실행 방법
```sh
./array_bounds
```
## 예상 관찰 결과
index 4는 마지막 값을 출력하고 index 5는 접근 없이 invalid로 처리된다.
## 확인 포인트
범위 검사 전에 array subscript를 평가하지 않는가?
## 추가 실습
- ★ index 0 - ★★ -1과 5 거부 - ★★★ 안전성 표
## 완료 기준
- [ ] 경고 없음 - [ ] invalid 접근 없음 - [ ] UB 설명
