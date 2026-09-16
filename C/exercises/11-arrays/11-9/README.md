# 11-9 실습: 역순 출력

이론: [note](../../../notes/11-arrays/11-9-reverse-output.md)
## 실습 목적
unsigned index를 안전하게 감소시키며 배열을 역순으로 읽는다.
## 작성할 파일
`reverse_output.c`
## 해야 할 일
10, 20, 30, 40, 50을 가진 배열을 50부터 10까지 출력한다.
## 사용할 개념
`size_t`, count, reverse traversal, `i - 1`, unsigned boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic reverse_output.c -o reverse_output
```
## 실행 방법
```sh
./reverse_output
```
## 예상 관찰 결과
50, 40, 30, 20, 10이 순서대로 출력된다.
## 확인 포인트
index 5나 unsigned underflow 값을 subscript로 사용하지 않는가?
## 추가 실습
- ★ 세 element - ★★ index 함께 출력 - ★★★ 잘못된 감소 loop 분석
## 완료 기준
- [ ] 경고 없음 - [ ] 역순 정확 - [ ] unsigned 경계 설명
