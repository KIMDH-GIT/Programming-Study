# 18-13 실습: memory leak 방지
이론: [note](../../../notes/18-dynamic-memory/18-13-memory-leak.md)

## 실습 목적
여러 allocation의 정상·부분 실패 cleanup을 작성한다.
## 작성할 파일
`leak_free_paths.c`
## 해야 할 일
두 `int`를 각각 allocation하고 어느 하나가 실패해도 성공한 allocation을 정리한다. 정상 경로도 둘 다 free한다.
## 사용할 개념
resource leak, partial failure, `free(NULL)`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic leak_free_paths.c -o leak_free_paths
```
## 실행 방법
```sh
./leak_free_paths
```
## 예상 관찰 결과
정상 종료하며 모든 allocation에 cleanup이 있다.
## 확인 포인트
leak과 UB를 같은 분류로 쓰지 않는다.
## 추가 실습
- ★ resource ledger
- ★★ reverse cleanup order
- ★★★ lost pointer analysis
## 완료 기준
모든 success allocation에 reachable free가 있다.
