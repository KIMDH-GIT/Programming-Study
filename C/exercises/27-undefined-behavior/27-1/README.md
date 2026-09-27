# 27-1 실습: Undefined Behavior 분류
이론: [note](../../../notes/27-undefined-behavior/27-1-undefined-behavior.md)
## 실습 목적
C17의 네 behavior 범주, constraint diagnostic 축, 도구 관찰을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
두 원소 배열을 만들고 index 범위를 검사한 뒤에만 읽는다. UB 분석 사례는 네 behavior 범주로 분류하고 constraint violation 여부는 별도 열에 표시한다.
## 사용할 개념
defined behavior, UB, unspecified, implementation-defined, constraint, diagnostic.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o ub_intro
```
## 실행 방법
UB 코드 자체는 실행하지 않는다. 범위 검사한 defined version만 실행한다.
```sh
./ub_intro
```
## 예상 관찰 결과
유효한 원소 `20`이 출력된다.
## 확인 포인트
compile 성공·warning 부재·정상 종료를 UB-free 증명으로 쓰지 않는다.
## 추가 실습
- ★ 사례 다섯 개를 분류한다.
- ★★ diagnostic requirement 열을 추가한다.
- ★★★ C17과 OS 관찰을 분리한다.
## 완료 기준
네 behavior 범주와 constraint diagnostic 축을 구분하고 defined code만 실행한다.
