# 7-4 실습: 비교 연산자
이론: [note](../../../notes/07-operators/7-4-comparison-operators.md)
## 실습 목적
비교 결과형과 값을 확인한다.
## 작성할 파일
`comparison.c`
## 해야 할 일
여섯 비교 중 세 가지 이상을 출력한다.
## 사용할 개념
equality, relational, `int` 0/1.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic comparison.c -o comparison
```
## 실행 방법
```sh
./comparison
```
## 예상 관찰 결과
0과 1이 표시된다.
## 확인 포인트
- `=`를 비교로 쓰지 않았는가?
- 결과형을 정확히 적었는가?
## 추가 실습
- ★ 기초: 정수 비교.
- ★★ 응용: 혼합형.
- ★★★ 도전: signed/unsigned.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 결과값을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
