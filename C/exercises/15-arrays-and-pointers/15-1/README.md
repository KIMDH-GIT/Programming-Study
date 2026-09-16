# 15-1 실습: 배열 이름과 첫 원소 주소

이론: [note](../../../notes/15-arrays-and-pointers/15-1-array-name-first-element.md)
## 실습 목적
array expression conversion과 first element address를 비교한다.
## 작성할 파일
`array_first_element.c`
## 해야 할 일
`values`, `&values[0]`을 `%p`로 출력하고 converted pointer로 첫 값을 읽는다.
## 사용할 개념
array object, array-to-pointer conversion, first element, `%p`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_first_element.c -o array_first_element
```
## 실행 방법
```sh
./array_first_element
```
## 예상 관찰 결과
두 pointer 표현이 같고 first element 값이 출력된다.
## 확인 포인트
array 자체를 pointer variable이라고 설명하지 않는가?
## 추가 실습
- ★ double array - ★★ type 표 - ★★★ context 분류
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] first value 정확 - [ ] conversion 설명
