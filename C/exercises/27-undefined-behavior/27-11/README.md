# 27-11 실습: compile 성공과 정확성
이론: [note](../../../notes/27-undefined-behavior/27-11-compile-success-vs-correctness.md)
## 실습 목적
library precondition을 지킨 program을 작성한다.
## 작성할 파일
- `main.c`
## 해야 할 일
modifiable array, safe ctype argument, overlap-safe `memmove`, 맞는 format을 사용한다.
## 사용할 개념
diagnostic, format contract, ctype domain, string literal, overlap.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o correctness
```
## 실행 방법
mismatched format과 literal modification은 실행하지 않는다.
```sh
./correctness
```
## 예상 관찰 결과
`HHell`이 출력된다.
## 확인 포인트
strict warning 통과만이 아니라 각 library precondition을 직접 확인한다.
## 추가 실습
- ★ `%p`와 `(void *)`를 올바르게 사용한다.
- ★★ format type 대응표를 만든다.
- ★★★ library contract checklist를 만든다.
## 완료 기준
compile 성공 근거와 semantic correctness 근거를 각각 설명한다.
