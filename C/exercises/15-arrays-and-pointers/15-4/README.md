# 15-4 실습: `&arr[i]`와 `arr + i`

이론: [note](../../../notes/15-arrays-and-pointers/15-4-element-address.md)
## 실습 목적
두 element pointer expressions의 관계를 확인한다.
## 작성할 파일
`element_addresses.c`
## 해야 할 일
모든 valid indices에서 `&arr[i]`, `arr+i`, dereferenced value를 출력한다.
## 사용할 개념
element address, pointer addition, `%p`, valid dereference.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic element_addresses.c -o element_addresses
```
## 실행 방법
```sh
./element_addresses
```
## 예상 관찰 결과
각 index의 두 pointer 표현이 같고 element value가 출력된다.
## 확인 포인트
one-past를 출력할 수 있어도 dereference하지 않는다는 점을 아는가?
## 추가 실습
- ★ char array - ★★ double array - ★★★ 구현 가정 표
## 완료 기준
- [ ] 경고 없음 - [ ] pointer 관계 확인 - [ ] bounds 준수
