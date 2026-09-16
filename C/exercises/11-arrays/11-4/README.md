# 11-4 실습: 연속 메모리 배치와 원소 주소

이론: [note](../../../notes/11-arrays/11-4-contiguous-layout-addresses.md)
## 실습 목적
배열 element의 연속 배치를 주소 출력으로 관찰한다.
## 작성할 파일
`array_addresses.c`
## 해야 할 일
4개 `int` element의 값과 주소를 출력하고 element 하나의 `sizeof`도 출력한다.
## 사용할 개념
연속 배치, element address, `%p`, `(void *)`, `sizeof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_addresses.c -o array_addresses
```
## 실행 방법
```sh
./array_addresses
```
## 예상 관찰 결과
index 순서의 주소가 `sizeof(int)`에 대응하는 간격으로 관찰된다. 숫자 주소 자체는 환경마다 다를 수 있다.
## 확인 포인트
관찰 결과와 C17 보장을 구분하고 `int` 크기를 고정 가정하지 않았는가?
## 추가 실습
- ★ `char` 배열 - ★★ 값과 주소 한 줄 출력 - ★★★ 표준/구현 표
## 완료 기준
- [ ] 경고 없음 - [ ] 네 주소 출력 - [ ] 배열과 pointer 구분
