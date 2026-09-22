# 19-2 실습: 구조체 선언·초기화와 `.`
이론: [note](../../../notes/19-structures/19-2-declaration-initialization-member-access.md)

## 실습 목적
구조체 초기화, member access, 전체 assignment를 확인한다.
## 작성할 파일
`point_initialization.c`
## 해야 할 일
positional object와 partial object를 만들고 전체 대입한 뒤 복사본의 한 member를 수정해 두 object를 출력한다.
## 사용할 개념
aggregate initialization, `.`, assignment, designated initializer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic point_initialization.c -o point_initialization
```
## 실행 방법
```sh
./point_initialization
```
## 예상 관찰 결과
partial member는 0이며 복사본 수정이 원본에 영향을 주지 않는다.
## 확인 포인트
`==`나 `memcmp`로 구조체 value equality를 검사하지 않는다.
## 추가 실습
- ★ designated initialization을 쓴다.
- ★★ member별 비교를 한다.
- ★★★ 배열 member가 있는 구조체 assignment를 관찰한다.
## 완료 기준
초기화와 assignment 결과를 예측하고 warning 없이 확인한다.
