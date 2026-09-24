# 24-6 실습: 여러 object file link
이론: [note](../../../notes/24-multi-file-programs/24-6-linking-multiple-object-files.md)
## 실습 목적
세 translation units의 object files를 한 실행 파일로 link한다.
## 작성할 파일
- `main.c`
- `calculator.c`
- `calculator.h`
- `output.c`
- `output.h`
## 해야 할 일
각 source를 `-c`로 compile하고 세 object files를 link한다.
## 사용할 개념
external reference, definition, object file, linker.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c calculator.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c output.c
gcc main.o calculator.o output.o -o calculator_app
```
## 실행 방법
```sh
./calculator_app
```
## 예상 관찰 결과
`result: 12`가 출력된다.
## 확인 포인트
one-command build와 separate compilation이 같은 translation units를 처리할 수 있음을 설명한다.
## 추가 실습
- ★ 한 명령으로 build한다.
- ★★ object file 하나를 빼고 link diagnostic을 관찰한다.
- ★★★ symbol 제공 관계를 표로 만든다.
## 완료 기준
세 compile과 한 link가 성공하고 결과가 정확하다.
