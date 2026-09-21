# 18-2 실습: automatic과 allocated lifetime
이론: [note](../../../notes/18-dynamic-memory/18-2-automatic-and-allocated-lifetime.md)

## 실습 목적
pointer와 pointed-to object lifetime을 분리한다.
## 작성할 파일
`allocated_lifetime.c`
## 해야 할 일
한 `int`를 allocation해 사용·해제하고 pointer에 NULL을 저장한다. alias가 자동 변경되지 않음을 주석으로 설명한다.
## 사용할 개념
object lifetime, owner pointer, null pointer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic allocated_lifetime.c -o allocated_lifetime
```
## 실행 방법
```sh
./allocated_lifetime
```
## 예상 관찰 결과
초기화한 값이 한 번 출력된다.
## 확인 포인트
free 후 old pointer를 읽거나 비교하지 않는다.
## 추가 실습
- ★ timeline 작성
- ★★ alias diagram 작성
- ★★★ ownership contract 작성
## 완료 기준
모든 access가 free 전이고 allocation이 정확히 한 번 해제된다.
