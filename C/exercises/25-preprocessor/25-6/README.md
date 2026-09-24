# 25-6 실습: macro 인자 중복 평가
이론: [note](../../../notes/25-preprocessor/25-6-macro-argument-multiple-evaluation.md)
## 실습 목적
safe macro usage와 unsequenced UB 분석을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
side-effect-free value로 `SQUARE`를 실행하고 `SQUARE(i++)`는 expansion만 분석한다.
## 사용할 개념
multiple evaluation, side effect, sequencing, undefined behavior.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o safe_macro_app
```
## 실행 방법
```sh
./safe_macro_app
```
## 예상 관찰 결과
정상 예제는 `16`을 출력하며 UB 사례는 실행하지 않는다.
## 확인 포인트
multiple evaluation과 unsequenced UB를 같은 말로 일반화하지 않는다.
## 추가 실습
- ★ 다른 constant input을 사용한다.
- ★★ `MIN`의 평가 횟수를 분석한다.
- ★★★ typed function과 비교한다.
## 완료 기준
정상 실행은 통과하고 위험 expansion의 UB 원인을 정확히 설명한다.
