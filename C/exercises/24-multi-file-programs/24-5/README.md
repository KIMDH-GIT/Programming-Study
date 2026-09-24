# 24-5 실습: 개별 compile과 object file
이론: [note](../../../notes/24-multi-file-programs/24-5-separate-compilation-and-object-files.md)
## 실습 목적
각 translation unit을 개별 compile하고 나중에 link한다.
## 작성할 파일
- `main.c`
- `counter.c`
- `counter.h`
## 해야 할 일
두 `.c`를 각각 `-c`로 compile하여 `main.o`, `counter.o`를 만들고 link한다.
## 사용할 개념
separate compilation, GCC `-c`, object file, link.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c counter.c
gcc main.o counter.o -o counter_app
```
## 실행 방법
```sh
./counter_app
```
## 예상 관찰 결과
`10`이 출력된다.
## 확인 포인트
C object와 `.o` object file을 같은 뜻으로 쓰지 않는다.
## 추가 실습
- ★ object file 이름을 직접 정한다.
- ★★ 한 source 변경 시 rebuild 범위를 설명한다.
- ★★★ build 단계별 input·output 표를 만든다.
## 완료 기준
개별 compile 두 번과 link 한 번이 성공하고 프로그램이 실행된다.
