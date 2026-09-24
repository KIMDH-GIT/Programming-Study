# 26-7 실습: global·static·local·heap 객체 분석
이론: [note](../../../notes/26-memory-structure/26-7-object-category-analysis.md)
## 실습 목적
편의 용어를 scope·linkage·duration·lifetime으로 분해한다.
## 작성할 파일
- `main.c`
## 해야 할 일
다섯 종류 object를 포함한 예제를 작성하고 표로 분류한다.
## 사용할 개념
file scope, block scope, linkage, static, automatic, allocated duration.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o analysis_app
```
## 실행 방법
```sh
./analysis_app
```
## 예상 관찰 결과
`1 2 4 4`가 출력된다.
## 확인 포인트
file static과 block static을 같은 의미로 설명하지 않는다.
## 추가 실습
- ★ saved를 두 번 갱신한다.
- ★★ tentative definition을 추가한다.
- ★★★ optimization을 설명한다.
## 완료 기준
각 object의 네 속성을 정확히 분류한다.
