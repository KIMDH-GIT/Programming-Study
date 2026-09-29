# 31-10 실습: padding과 member offset
이론: [note](../../../notes/31-system-embedded-c/31-10-padding-and-member-offset.md)
## 실습 목적
member order와 object extent를 검사하고 exact padding 수를 고정하지 않는다.
## 작성할 파일
- `main.c`
## 해야 할 일
세 member의 offset 순서와 `sizeof`가 마지막 member를 포함하는지 검사한다. raw bytes를 protocol/device format으로 사용하지 않는다.
## 사용할 개념
struct member order, padding, `offsetof`, `sizeof`, serialization boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o padding
```
## 실행 방법
```sh
./padding
```
## 예상 관찰 결과
`ordered=1 size_covers_last=1`을 출력한다.
## 확인 포인트
padding 없음, compiler 간 동일 layout, pointer 8 bytes 같은 고정 가정을 쓰지 않는다.
## 추가 실습
- ★ member 순서를 바꾼다.
- ★★ concrete offsets를 host observation으로 기록한다.
- ★★★ explicit external encoding을 설계한다.
## 완료 기준
portable invariant가 통과하고 layout number를 C17 보장으로 과장하지 않는다.
