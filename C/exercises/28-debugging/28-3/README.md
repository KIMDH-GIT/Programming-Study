# 28-3 실습: `-g -Og` debug build
이론: [note](../../../notes/28-debugging/28-3-g-og-debug-build.md)
## 실습 목적
debug information과 optimization level을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
같은 source를 `-g -Og`와 `-g -O0`로 build한다.
## 사용할 개념
debug information, optimization level, source-level debugging.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o app_og
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -O0 main.c -o app_o0
```
## 실행 방법
```sh
./app_og
./app_o0
```
## 예상 관찰 결과
두 binary 모두 `42`를 출력한다.
## 확인 포인트
`-g`, `-Og`, `-O0`을 같은 option으로 설명하지 않는다.
## 추가 실습
- ★ debug section을 관찰한다.
- ★★ GDB에서 local variable을 비교한다.
- ★★★ optimized-out의 의미를 설명한다.
## 완료 기준
두 build가 성공하고 option 목적을 구분한다.
