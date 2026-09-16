# 16-5 실습: `swap(int *a, int *b)`

이론: [note](../../../notes/16-pointers-and-functions/16-5-swap.md)
## 실습 목적
값 swap 실패와 pointer swap 성공을 비교한다.
## 작성할 파일
`swap.c`
## 해야 할 일
두 swap versions를 호출하고 caller values 전후를 출력한다.
## 사용할 개념
value parameter, pointer parameter, indirection, temporary variable.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic swap.c -o swap
```
## 실행 방법
```sh
./swap
```
## 예상 관찰 결과
value version 뒤 10 20, pointer version 뒤 20 10이다.
## 확인 포인트
두 versions 모두 pass-by-value로 설명하는가?
## 추가 실습
- ★ double swap - ★★ same object - ★★★ diagrams
## 완료 기준
- [ ] 경고 없음 - [ ] 두 결과 정확 - [ ] a/*a 구분
