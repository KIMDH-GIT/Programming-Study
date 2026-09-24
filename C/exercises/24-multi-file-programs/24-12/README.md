# 24-12 실습: Part 24 종합 복습
이론: [note](../../../notes/24-multi-file-programs/24-12-part-24-review.md)
## 실습 목적
public API, private helper, separate compilation, link를 종합한다.
## 작성할 파일
- `main.c`
- `app.c`
- `app.h`
## 해야 할 일
public function과 source-local static helper를 작성하고 두 object files를 개별 compile·link한다.
## 사용할 개념
translation unit, declaration, definition, internal linkage, external linkage, object file.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -c app.c
gcc main.o app.o -o app
```
## 실행 방법
```sh
./app
```
## 예상 관찰 결과
`12`가 출력된다.
## 확인 포인트
preprocessing, compile, link, load, execution의 책임을 섞지 않는다.
## 추가 실습
- ★ 한 명령으로도 build한다.
- ★★ implementation object를 뺀 link failure를 분석한다.
- ★★★ 이름과 artifact를 scope·linkage·종류별로 분류한다.
## 완료 기준
strict C17 separate build·실행이 성공하고 핵심 용어를 정확히 설명한다.
