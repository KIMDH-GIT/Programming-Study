# 22-5 실습: callback
이론: [note](../../../notes/22-function-pointers/22-5-callback.md)
## 실습 목적
calculator에 function pointer callback을 pass-by-value로 전달한다.
## 작성할 파일
`calculator_callback.c`
## 해야 할 일
add·subtract·multiply와 `calculate`를 작성해 같은 signature callbacks를 호출한다.
## 사용할 개념
callback, function pointer parameter, pass-by-value, signature compatibility.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic calculator_callback.c -o calculator_callback
```
## 실행 방법
```sh
./calculator_callback
```
## 예상 관찰 결과
같은 operands에 대해 선택한 세 operations의 결과가 각각 출력된다.
## 확인 포인트
callback을 call-by-reference나 captured environment라고 설명하지 않는다.
## 추가 실습
- ★ remainder callback을 추가한다.
- ★★ operator character로 callback을 선택한다.
- ★★★ pointer value 전달 흐름을 그린다.
## 완료 기준
compatible callbacks만 전달하고 각 결과를 warning 없이 확인한다.
