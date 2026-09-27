# 27-12 실습: Part 27 종합 복습
이론: [note](../../../notes/27-undefined-behavior/27-12-part-27-review.md)
## 실습 목적
대표 UB를 실행 전에 차단하고 행동 범주를 종합한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`safe_average`를 구현하고 null·empty·overflow 조건을 먼저 검사한다.
## 사용할 개념
behavior categories, array bound, pointer validity, signed range, precondition.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part27_review
```
## 실행 방법
UB 분석 코드는 실행하지 않는다. defined review program만 실행한다.
```sh
./part27_review
```
## 예상 관찰 결과
`20`이 출력된다.
## 확인 포인트
C17·compiler·sanitizer·OS·ISA 층을 구분한다.
## 추가 실습
- ★ 각 Step의 safe counterpart를 정리한다.
- ★★ diagnostic과 optional warning을 비교한다.
- ★★★ unexecuted path가 sanitizer에 남기는 한계를 설명한다.
## 완료 기준
12개 Step의 핵심 분류와 안전한 대응을 설명한다.
