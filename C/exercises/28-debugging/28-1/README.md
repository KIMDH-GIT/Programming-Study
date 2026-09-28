# 28-1 실습: GCC C17 warning options
이론: [note](../../../notes/28-debugging/28-1-gcc-c17-warning-options.md)
## 실습 목적
C dialect와 warning option의 역할을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
세 정수의 합을 출력하고 각 GCC option의 역할을 표로 쓴다.
## 사용할 개념
`-std=c17`, `-Wall`, `-Wextra`, `-Wpedantic`, `-Werror`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
```
## 실행 방법
```sh
./warning_options
```
## 예상 관찰 결과
warning 없이 build되고 `6`이 출력된다.
## 확인 포인트
option 통과를 correctness proof라고 설명하지 않는다.
## 추가 실습
- ★ option을 하나씩 추가한다.
- ★★ GCC manual의 warning 집합을 확인한다.
- ★★★ required diagnostic과 warning을 구분한다.
## 완료 기준
네 option의 역할을 정확히 설명하고 정상 실행한다.
