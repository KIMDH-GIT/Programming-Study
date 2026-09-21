# 18-20 실습: Part 18 종합 복습
이론: [note](../../../notes/18-dynamic-memory/18-20-part-18-review.md)

## 실습 목적
동적 메모리의 size·failure·lifetime·ownership 규칙을 종합한다.
## 작성할 파일
`part18_review.c`
## 해야 할 일
작은 dynamic array를 overflow guard 후 allocation한다. 초기화하고 temporary pointer로 확장한 뒤 새 element를 저장·출력·free한다.
## 사용할 개념
`malloc`, `realloc`, temporary pointer, bounds, ownership.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part18_review.c -o part18_review
```
## 실행 방법
```sh
./part18_review
```
## 예상 관찰 결과
확장된 배열의 마지막 값이 출력된다.
## 확인 포인트
overflow·failure·uninitialized read·old alias·leak이 없다.
## 추가 실습
- ★ error taxonomy
- ★★ ownership diagram
- ★★★ API contract table
## 완료 기준
Part 18의 정상 흐름을 모두 지키고 C17 warning이 없다.
