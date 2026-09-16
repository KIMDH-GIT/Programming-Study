# 6-3 실습: 정수·실수 변환
이론: [note](../../../notes/06-type-conversions/6-3-integer-floating-conversion.md)
## 실습 목적
양방향 변환 규칙을 관찰한다.
## 작성할 파일
`integer_floating.c`
## 해야 할 일
정수→실수와 ±실수→정수를 출력한다.
## 사용할 개념
정밀도, 0 방향 절단, 범위.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_floating.c -o integer_floating
```
## 실행 방법
```sh
./integer_floating
```
## 예상 관찰 결과
42.0, 3, -3.
## 확인 포인트
- 절단을 내림과 구별했는가?
- 범위 밖 값을 쓰지 않았는가?
## 추가 실습
- ★ 기초: 양수.
- ★★ 응용: 음수.
- ★★★ 도전: 정밀도 한계.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 결과를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
