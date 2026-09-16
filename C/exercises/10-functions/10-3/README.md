# 10-3 실습: 매개변수와 인수

이론: [note](../../../notes/10-functions/10-3-parameters-arguments.md)
## 실습 목적
parameter와 argument의 위치·역할을 구분한다.
## 작성할 파일
`parameters_arguments.c`
## 해야 할 일
두 `int` parameters 중 큰 값을 반환하는 `larger`를 정의하고 `larger(7, 4)`를 호출한다.
## 사용할 개념
parameter, argument, 위치 대응, 반환값.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic parameters_arguments.c -o parameters_arguments
```
## 실행 방법
```sh
./parameters_arguments
```
## 예상 관찰 결과
`7`이 출력된다.
## 확인 포인트
definition의 이름과 call의 expression을 서로 다른 용어로 설명했는가?
## 추가 실습
- ★ 작은 값 - ★★ 세 수의 합 - ★★★ argument 순서 비교
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 7 - [ ] 두 용어 정확
