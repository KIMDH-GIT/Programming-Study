# 20-1 실습: `enum`과 열거 상수
이론: [note](../../../notes/20-enum-typedef-union/20-1-enum-and-enumeration-constants.md)

## 실습 목적
enumeration tag, constant, object와 값 계산 규칙을 구분한다.
## 작성할 파일
`enum_direction.c`
## 해야 할 일
생략값과 명시값이 섞인 `enum Direction`을 정의하고 모든 값을 출력한다.
## 사용할 개념
`enum`, tag, enumerator, compatible integer type.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic enum_direction.c -o enum_direction
```
## 실행 방법
```sh
./enum_direction
```
## 예상 관찰 결과
선언 규칙에 따라 0, 1, 명시값, 그 다음 값이 출력된다.
## 확인 포인트
enumerator type과 enum object type을 같은 것으로 설명하지 않는다.
## 추가 실습
- ★ 비연속 값을 만든다.
- ★★ 중복 값을 관찰한다.
- ★★★ `sizeof` 관찰값을 구현 결과로 기록한다.
## 완료 기준
값을 정확히 예측하고 warning 없이 실행한다.
