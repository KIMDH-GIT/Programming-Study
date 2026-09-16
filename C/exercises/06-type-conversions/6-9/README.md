# 6-9 실습: cast 위치
이론: [note](../../../notes/06-type-conversions/6-9-cast-position.md)
## 실습 목적
cast 전후 중간 결과를 비교한다.
## 작성할 파일
`cast_position.c`
## 해야 할 일
늦은 cast와 이른 cast를 출력한다.
## 사용할 개념
하위 식, 중간형, 괄호.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic cast_position.c -o cast_position
```
## 실행 방법
```sh
./cast_position
```
## 예상 관찰 결과
2.0과 2.5.
## 확인 포인트
- cast 범위를 설명했는가?
- 평가 순서로 오해하지 않았는가?
## 추가 실습
- ★ 기초: 다른 수.
- ★★ 응용: double 변수.
- ★★★ 도전: 중간형 표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 차이를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
