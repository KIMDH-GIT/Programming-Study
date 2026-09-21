# 18-7 실습: allocation failure
이론: [note](../../../notes/18-dynamic-memory/18-7-allocation-failure-and-null.md)

## 실습 목적
NULL 반환을 dereference 전에 처리한다.
## 작성할 파일
`allocation_failure.c`
## 해야 할 일
배열 allocation 뒤 failure branch에서 stderr 메시지와 nonzero status를 반환하고 success branch에서 사용·해제한다.
## 사용할 개념
allocation failure, null pointer, stderr.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic allocation_failure.c -o allocation_failure
```
## 실행 방법
```sh
./allocation_failure
```
## 예상 관찰 결과
정상 환경에서는 저장한 값이 출력된다.
## 확인 포인트
failure 자체와 null dereference UB를 구분한다.
## 추가 실습
- ★ error message 개선
- ★★ allocation-returning function
- ★★★ partial cleanup flow
## 완료 기준
모든 dereference가 NULL 검사 뒤에 있다.
