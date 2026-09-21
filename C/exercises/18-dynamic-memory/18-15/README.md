# 18-15 실습: double free와 NULL
이론: [note](../../../notes/18-dynamic-memory/18-15-double-free-null-dereference.md)

## 실습 목적
single free, `free(NULL)`, double free, null dereference를 구분한다.
## 작성할 파일
`single_free.c`
## 해야 할 일
allocation을 한 번 free하고 pointer에 NULL을 저장한 뒤 `free(NULL)`만 수행한다. UB fragments는 표로 분석한다.
## 사용할 개념
double free, null pointer, undefined behavior.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic single_free.c -o single_free
```
## 실행 방법
```sh
./single_free
```
## 예상 관찰 결과
정상 종료한다.
## 확인 포인트
NULL 대입과 deallocation을 같은 동작으로 설명하지 않는다.
## 추가 실습
- ★ valid free table
- ★★ single-owner contract
- ★★★ alias double-free analysis
## 완료 기준
double free나 NULL dereference를 실행하지 않는다.
