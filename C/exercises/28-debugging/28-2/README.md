# 28-2 실습: warning 원인 수정
이론: [note](../../../notes/28-debugging/28-2-reading-and-fixing-compiler-warnings.md)
## 실습 목적
diagnostic을 읽고 원인인 interface를 수정한다.
## 작성할 파일
- `warning.c`
- `main.c`
## 해야 할 일
unused parameter warning을 관찰한 뒤 parameter와 caller를 함께 수정한다.
## 사용할 개념
diagnostic location, warning option, interface contract.
## 컴파일 방법
관찰 command와 수정 후 strict command를 분리한다.
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic warning.c -o warning_app
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o fixed_warning
```
## 실행 방법
warning source는 실행하지 않고 수정한 program만 실행한다.
```sh
./fixed_warning
```
## 예상 관찰 결과
관찰 build에서 unused-parameter 계열 diagnostic이 발생하고 수정본은 `42`를 출력한다.
## 확인 포인트
diagnostic 전체 문장이 아니라 category와 controlling option을 확인한다.
## 추가 실습
- ★ 첫 warning 위치를 기록한다.
- ★★ cast suppression과 contract 수정 차이를 쓴다.
- ★★★ link error와 runtime failure를 분류한다.
## 완료 기준
expected diagnostic과 정상 PASS를 별도로 기록한다.
