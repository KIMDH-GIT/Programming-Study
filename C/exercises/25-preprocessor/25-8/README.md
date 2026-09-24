# 25-8 실습: header guard
이론: [note](../../../notes/25-preprocessor/25-8-header-guards.md)
## 실습 목적
같은 header가 direct·indirect 두 경로로 포함되는 상황을 guard로 처리한다.
## 작성할 파일
- `main.c`
- `app.c`
- `app.h`
- `common.h`
## 해야 할 일
두 headers에 project-prefixed guards를 만들고 nested include 구조를 compile한다.
## 사용할 개념
`#ifndef`, `#define`, `#endif`, repeated inclusion, translation unit.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c app.c -o guard_app
```
## 실행 방법
```sh
./guard_app
```
## 예상 관찰 결과
redefinition 없이 build되고 `42`가 출력된다.
## 확인 포인트
guard 범위를 program 전체가 아닌 같은 preprocessing translation unit으로 설명한다.
## 추가 실습
- ★ guard 이름을 검토한다.
- ★★ 두 translation units의 include 흐름을 그린다.
- ★★★ linker definition 문제와 비교한다.
## 완료 기준
direct·indirect repeated include가 strict C17에서 정상 build·실행된다.
