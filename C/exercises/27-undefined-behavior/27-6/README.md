# 27-6 실습: `NULL` pointer 검사
이론: [note](../../../notes/27-undefined-behavior/27-6-null-pointer-dereference.md)
## 실습 목적
nullable pointer를 역참조 전에 검사한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`read_value`를 작성해 null input/output pointer에서 실패를 반환한다.
## 사용할 개념
null pointer, valid object, dereference contract.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o null_check
```
## 실행 방법
null dereference 자체는 실행하지 않는다. 검사된 version만 실행한다.
```sh
./null_check
```
## 예상 관찰 결과
유효한 pointer에서는 `42`, null case에서는 실패 상태를 관찰한다.
## 확인 포인트
`NULL`을 반드시 machine address zero라고 설명하지 않는다.
## 추가 실습
- ★ null output pointer를 검사한다.
- ★★ allocation failure path와 연결한다.
- ★★★ API nullable contract를 문서화한다.
## 완료 기준
null path에서 역참조가 전혀 일어나지 않는다.
