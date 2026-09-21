# 18-10 실습: zero-size policy
이론: [note](../../../notes/18-dynamic-memory/18-10-zero-size-allocation.md)

## 실습 목적
zero count를 allocation 전에 명시적으로 처리한다.
## 작성할 파일
`zero_size_policy.c`
## 해야 할 일
count가 0이면 allocation 없이 종료하고, 양수면 정상 allocation·사용·free를 수행한다.
## 사용할 개념
zero-size implementation-defined behavior, application policy.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic zero_size_policy.c -o zero_size_policy
```
## 실행 방법
```sh
./zero_size_policy
```
## 예상 관찰 결과
설정한 양수 count 경로가 정상 종료한다.
## 확인 포인트
`malloc(0)`과 `realloc(p, 0)` 결과를 단정하지 않는다.
## 추가 실습
- ★ empty policy 문서화
- ★★ zero와 failure 구분
- ★★★ C17 option table
## 완료 기준
교육용 allocation은 size > 0이며 zero result를 dereference하지 않는다.
