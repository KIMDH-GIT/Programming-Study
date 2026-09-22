# 20-3 실습: enum 기반 state 표현
이론: [note](../../../notes/20-enum-typedef-union/20-3-enum-based-state.md)

## 실습 목적
검증된 state를 enum structure member에 저장한다.
## 작성할 파일
`student_state.c`
## 해야 할 일
세 상태를 정의하고 정수 후보가 named state인지 검사한 뒤 학생 record에 저장한다.
## 사용할 개념
enum object, structure member, integer conversion, validation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_state.c -o student_state
```
## 실행 방법
```sh
./student_state
```
## 예상 관찰 결과
허용 상태만 저장되고 범위 밖 입력은 거부된다.
## 확인 포인트
cast 자체를 validation으로 취급하지 않는다.
## 추가 실습
- ★ 상태 출력 함수를 만든다.
- ★★ invalid 값을 시험한다.
- ★★★ transition 표를 작성한다.
## 완료 기준
state object와 enumeration constant를 구분한다.
