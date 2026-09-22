# 19-7 실습: 구조체를 함수에 전달
이론: [note](../../../notes/19-structures/19-7-passing-structures-to-functions.md)
## 실습 목적
value·pointer parameter와 structure return을 비교한다.
## 작성할 파일
`point_functions.c`
## 해야 할 일
생성, 출력, 복사본 수정, 원본 수정 함수를 작성한다.
## 사용할 개념
pass-by-value, pointer parameter, `const`, return.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic point_functions.c -o point_functions
```
## 실행 방법
```sh
./point_functions
```
## 예상 관찰 결과
복사본 수정은 원본에 없고 pointer 함수 수정은 반영된다.
## 확인 포인트
local object 주소를 반환하지 않는다.
## 추가 실습
- ★ const 출력 함수를 쓴다.
- ★★ 합산 결과를 반환한다.
- ★★★ 두 방식의 contract를 문서화한다.
## 완료 기준
두 전달 방식의 관찰 결과를 정확히 예측한다.
