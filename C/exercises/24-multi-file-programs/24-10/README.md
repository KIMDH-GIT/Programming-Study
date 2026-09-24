# 24-10 실습: 중복 정의와 undefined reference
이론: [note](../../../notes/24-multi-file-programs/24-10-duplicate-definitions-and-undefined-reference.md)
## 실습 목적
compile success와 link failure를 실제 명령으로 구분한다.
## 작성할 파일
- `main.c`
- `math_utils.c`
- `math_utils.h`
## 해야 할 일
정상 build를 확인한 뒤 `main.o`만 link하여 expected failure를 관찰하고 정상 상태로 복구한다.
## 사용할 개념
compile diagnostic, link diagnostic, external reference, definition.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c math_utils.c -o math_app
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc main.o -o missing_definition_app
```
마지막 명령은 의도적으로 실패해야 한다.
## 실행 방법
```sh
./math_app
```
## 예상 관찰 결과
정상 program은 `7`을 출력하고, implementation을 뺀 link는 unresolved external diagnostic으로 실패한다.
## 확인 포인트
expected link failure를 정상 executable PASS와 분리한다.
## 추가 실습
- ★ 정상 link 명령으로 복구한다.
- ★★ duplicate external definition diagnostic을 관찰한다.
- ★★★ 단계별 오류 표를 만든다.
## 완료 기준
정상 실행과 의도적 link failure를 모두 원인과 함께 설명한다.
