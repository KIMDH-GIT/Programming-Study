# 27-2 실습: implementation-defined behavior
이론: [note](../../../notes/27-undefined-behavior/27-2-implementation-defined-behavior.md)
## 실습 목적
구현이 문서화하는 선택을 관찰한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`CHAR_MIN`과 `-8 >> 1`을 출력하고 현재 compiler 문서의 선택을 기록한다.
## 사용할 개념
implementation-defined behavior, plain `char`, signed right shift.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o impl_defined
```
## 실행 방법
```sh
./impl_defined
```
## 예상 관찰 결과
현재 구현의 선택이 출력되며 exact 값은 이식 가능한 예상값으로 고정하지 않는다.
## 확인 포인트
음수 signed right shift는 C17에서 UB가 아니다.
## 추가 실습
- ★ `CHAR_MAX`도 출력한다.
- ★★ compiler 문서를 인용한다.
- ★★★ 다른 target의 문서와 비교한다.
## 완료 기준
C17 분류와 현재 구현의 결과를 별도로 기록한다.
