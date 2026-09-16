# 11-8 실습: 최댓값과 최솟값

이론: [note](../../../notes/11-arrays/11-8-min-max.md)
## 실습 목적
첫 element를 후보로 두고 전체 배열을 비교한다.
## 작성할 파일
`array_min_max.c`
## 해야 할 일
`{-3, 7, 2, -8, 4}`에서 최댓값과 최솟값을 찾아 출력한다.
## 사용할 개념
nonempty array, initial candidate, comparison, traversal.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_min_max.c -o array_min_max
```
## 실행 방법
```sh
./array_min_max
```
## 예상 관찰 결과
최댓값 7, 최솟값 -8이 출력된다.
## 확인 포인트
후보를 0이 아니라 `values[0]`으로 시작하는가?
## 추가 실습
- ★ 모두 양수 - ★★ 모두 음수 - ★★★ 후보 변경 index
## 완료 기준
- [ ] 경고 없음 - [ ] 7과 -8 출력 - [ ] nonempty 전제 설명
