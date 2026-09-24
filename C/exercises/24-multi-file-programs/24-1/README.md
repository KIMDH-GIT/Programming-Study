# 24-1 실습: 기능별 source file 분리
이론: [note](../../../notes/24-multi-file-programs/24-1-feature-based-source-file-separation.md)
## 실습 목적
단일 파일 계산기를 공개 interface와 구현으로 분리한다.
## 작성할 파일
- `main.c`
- `calculator.c`
- `calculator.h`
## 해야 할 일
`calculator_add`의 선언은 header, 정의는 implementation source에 두고 `main.c`에서 호출한다.
## 사용할 개념
source 분리, 공개 선언, function definition, translation unit, link.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c calculator.c -o calculator_app
```
## 실행 방법
```sh
./calculator_app
```
## 예상 관찰 결과
`12`가 출력된다.
## 확인 포인트
두 `.c`가 각각 compile되고 header는 두 translation unit에 source text로 포함된다.
## 추가 실습
- ★ 뺄셈 함수를 추가한다.
- ★★ 공개 함수에 `calculator_` prefix를 쓴다.
- ★★★ 각 파일의 책임을 표로 만든다.
## 완료 기준
세 파일만 작성하여 strict C17 warning 없이 build·실행한다.
