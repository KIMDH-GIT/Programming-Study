# 26-6 실습: heap과 동적 객체
이론: [note](../../../notes/26-memory-structure/26-6-heap-and-dynamic-objects.md)
## 실습 목적
allocated storage duration과 heap 구현 용어를 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
allocation failure를 검사하고 값을 저장·출력·해제한다.
## 사용할 개념
`malloc`, `free`, allocated storage duration, lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o allocated_app
```
## 실행 방법
```sh
./allocated_app
```
## 예상 관찰 결과
`26`이 출력된다.
## 확인 포인트
allocated duration을 heap이라는 C17 category로 부르지 않는다.
## 추가 실습
- ★ 배열을 할당한다.
- ★★ 두 object durations를 비교한다.
- ★★★ allocator 전략을 조사한다.
## 완료 기준
NULL 검사와 해제를 포함해 warning 없이 실행한다.
