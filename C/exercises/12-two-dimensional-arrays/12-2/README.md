# 12-2 실습: 2차원 배열 초기화

이론: [note](../../../notes/12-two-dimensional-arrays/12-2-initialization.md)
## 실습 목적
nested initializer와 부분 초기화 결과를 확인한다.
## 작성할 파일
`matrix_initialization.c`
## 해야 할 일
2x3 배열을 `{{1, 2}, {3}}`으로 초기화하고 여섯 값을 표 형태로 출력한다.
## 사용할 개념
nested initializer, row, column, partial initialization, zero initialization.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_initialization.c -o matrix_initialization
```
## 실행 방법
```sh
./matrix_initialization
```
## 예상 관찰 결과
첫 row는 `1 2 0`, 둘째 row는 `3 0 0`이다.
## 확인 포인트
명시되지 않은 값을 우연한 0이라고 설명하지 않는가?
## 추가 실습
- ★ 2x2 완전 초기화 - ★★ 3x3 `{0}` - ★★★ 대응표
## 완료 기준
- [ ] 경고 없음 - [ ] 여섯 값 정확 - [ ] 초기화 규칙 설명
