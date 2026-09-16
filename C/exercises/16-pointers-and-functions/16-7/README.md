# 16-7 실습: 배열 길이를 별도로 전달하는 이유

이론: [note](../../../notes/16-pointers-and-functions/16-7-pass-array-length.md)
## 실습 목적
pointer argument와 element count를 함께 전달한다.
## 작성할 파일
`array_length_parameter.c`
## 해야 할 일
caller에서 count를 계산해 array sum function에 전달한다.
## 사용할 개념
adjusted pointer parameter, `size_t`, count contract, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_length_parameter.c -o array_length_parameter
```
## 실행 방법
```sh
./array_length_parameter
```
## 예상 관찰 결과
count 5와 sum 150이 출력된다.
## 확인 포인트
function 안의 parameter `sizeof`로 count를 계산하지 않는가?
## 추가 실습
- ★ three values - ★★ count 0 - ★★★ contract analysis
## 완료 기준
- [ ] 경고 없음 - [ ] 5/150 정확 - [ ] count 별도 전달
