# 26-1 실습: C storage duration과 OS 배치의 구분
이론: [note](../../../notes/26-memory-structure/26-1-storage-duration-vs-os-placement.md)
## 실습 목적
objects를 scope·linkage·storage duration으로 분류한다.
## 작성할 파일
- `main.c`
## 해야 할 일
global, automatic local, block static, allocated storage를 포함한 예제를 작성한다.
## 사용할 개념
automatic, static, allocated storage duration, scope, linkage, lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o duration_app
```
## 실행 방법
```sh
./duration_app
```
## 예상 관찰 결과
`12 13`이 출력된다.
## 확인 포인트
automatic을 stack, allocated를 heap이라는 C17 정의로 설명하지 않는다.
## 추가 실습
- ★ 호출을 한 번 늘린다.
- ★★ file-scope static을 추가한다.
- ★★★ 언어 semantics와 OS 배치를 분리한다.
## 완료 기준
각 object의 네 가지 속성을 구분하고 warning 없이 실행한다.
