# 24-2 실습: declaration과 definition
이론: [note](../../../notes/24-multi-file-programs/24-2-declarations-and-definitions.md)
## 실습 목적
공유 prototype과 실제 function definition을 분리한다.
## 작성할 파일
- `main.c`
- `temperature.c`
- `temperature.h`
## 해야 할 일
`celsius_to_fahrenheit` declaration을 header에, definition을 source에 작성한다.
## 사용할 개념
declaration, definition, function prototype, compatible type.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c temperature.c -o temperature_app
```
## 실행 방법
```sh
./temperature_app
```
## 예상 관찰 결과
`68.0`이 출력된다.
## 확인 포인트
두 `.c`가 같은 header를 include하며 prototype을 복붙하지 않는다.
## 추가 실습
- ★ 역변환 함수를 추가한다.
- ★★ parameter names를 읽기 좋게 정한다.
- ★★★ incompatible definition의 compiler diagnostic을 관찰하고 복구한다.
## 완료 기준
선언과 정의가 일치하고 warning 없이 실행된다.
