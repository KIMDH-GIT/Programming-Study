# 25-1 실습: 전처리 지시문과 결과
이론: [note](../../../notes/25-preprocessor/25-1-preprocessing-directives-and-output.md)
## 실습 목적
preprocessing과 runtime execution을 실제 명령으로 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`MESSAGE` object-like macro를 정의하고 `gcc -E` 결과와 실행 출력을 각각 확인한다.
## 사용할 개념
preprocessing directive, preprocessing token, GCC `-E`, translation pipeline.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o preprocessor_app
```
## 실행 방법
```sh
./preprocessor_app
```
## 예상 관찰 결과
preprocessed output에 replacement string이 보이고 실행하면 `preprocessing`이 출력된다.
## 확인 포인트
`-E` 성공과 compile·link 성공을 같은 것으로 말하지 않는다.
## 추가 실습
- ★ message를 바꾼다.
- ★★ line markers와 C statements를 구분한다.
- ★★★ translation 단계 표를 만든다.
## 완료 기준
preprocessing 결과를 관찰하고 strict C17 build·실행까지 성공한다.
