# 6-1 실습: implicit conversion
이론: [note](../../../notes/06-type-conversions/6-1-implicit-conversion.md)
## 실습 목적
정수·실수 자동 변환을 관찰한다.
## 작성할 파일
`implicit_conversion.c`
## 해야 할 일
정수→실수, 실수→정수 초기화 결과를 출력한다.
## 사용할 개념
초기화, 값, 대상형, 정보 손실.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic implicit_conversion.c -o implicit_conversion
```
## 실행 방법
```sh
./implicit_conversion
```
## 예상 관찰 결과
7.0과 3이 보인다.
## 확인 포인트
- 변환 방향을 설명했는가?
- 초기화와 대입을 구별했는가?
## 추가 실습
- ★ 기초: 양수를 변환한다.
- ★★ 응용: 음수 실수를 변환한다.
- ★★★ 도전: 손실표를 만든다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 두 변환을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
