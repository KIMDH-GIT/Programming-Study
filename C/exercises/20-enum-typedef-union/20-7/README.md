# 20-7 실습: `sizeof`, `_Alignof`, `offsetof`
이론: [note](../../../notes/20-enum-typedef-union/20-7-sizeof-alignof-offsetof.md)

## 실습 목적
size, alignment, member offset을 현재 구현에서 관찰한다.
## 작성할 파일
`layout_observe.c`
## 해야 할 일
structure와 union의 `sizeof`, `_Alignof`, structure member `offsetof`를 출력한다.
## 사용할 개념
`size_t`, `%zu`, `_Alignof`, `offsetof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic layout_observe.c -o layout_observe
```
## 실행 방법
```sh
./layout_observe
```
## 예상 관찰 결과
현재 compiler/ABI가 선택한 숫자가 출력된다.
## 확인 포인트
관찰값을 C17 고정값으로 쓰지 않는다.
## 추가 실습
- ★ member 순서를 바꾼다.
- ★★ union과 비교한다.
- ★★★ 다른 compiler 결과를 비교한다.
## 완료 기준
세 연산의 의미를 구분하고 warning 없이 실행한다.
