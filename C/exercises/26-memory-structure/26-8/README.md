# 26-8 실습: `%p`로 주소 관찰
이론: [note](../../../notes/26-memory-structure/26-8-observing-addresses-with-percent-p.md)
## 실습 목적
valid object addresses를 portable format으로 출력한다.
## 작성할 파일
- `main.c`
## 해야 할 일
static, automatic, allocated object pointers를 `%p`로 출력한다.
## 사용할 개념
object pointer, `void *`, `%p`, PIE, ASLR.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o address_app
```
## 실행 방법
```sh
./address_app
```
## 예상 관찰 결과
세 implementation-defined address strings와 sentinel line이 출력된다.
## 확인 포인트
주소 값·순서를 고정하지 않는다.
## 추가 실습
- ★ 두 번 실행한다.
- ★★ variability 원인을 분류한다.
- ★★★ `nm` 관찰과 비교한다.
## 완료 기준
valid `%p` usage와 비이식성 설명을 모두 만족한다.
