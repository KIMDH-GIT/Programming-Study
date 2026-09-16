# 6-5 실습: usual arithmetic conversions
이론: [note](../../../notes/06-type-conversions/6-5-usual-arithmetic-conversions.md)
## 실습 목적
서로 다른 산술형의 공통형을 판별한다.
## 작성할 파일
`usual_conversions.c`
## 해야 할 일
`int + double`을 계산하고 과정표를 쓴다.
## 사용할 개념
공통형, promotion, 결과형.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic usual_conversions.c -o usual_conversions
```
## 실행 방법
```sh
./usual_conversions
```
## 예상 관찰 결과
3.5.
## 확인 포인트
- 중간형을 설명했는가?
- 대입과 계산을 구별했는가?
## 추가 실습
- ★ 기초: float+double.
- ★★ 응용: 정수 rank.
- ★★★ 도전: 결정표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 공통형을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
