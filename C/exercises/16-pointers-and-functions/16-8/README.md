# 16-8 실습: 함수에서 배열 원소 변경

이론: [note](../../../notes/16-pointers-and-functions/16-8-modify-array-elements.md)
## 실습 목적
adjusted pointer parameter로 caller elements를 bounds 안에서 수정한다.
## 작성할 파일
`modify_array_elements.c`
## 해야 할 일
four-element array를 function에서 1씩 증가시키고 caller에서 출력한다.
## 사용할 개념
array conversion, parameter adjustment, element assignment, count.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic modify_array_elements.c -o modify_array_elements
```
## 실행 방법
```sh
./modify_array_elements
```
## 예상 관찰 결과
2, 3, 4, 5가 출력된다.
## 확인 포인트
call by reference가 아니라 copied pointer를 통한 element access로 설명하는가?
## 추가 실습
- ★ double - ★★ negatives only - ★★★ flow diagram
## 완료 기준
- [ ] 경고 없음 - [ ] 2~5 정확 - [ ] bounds 안전
