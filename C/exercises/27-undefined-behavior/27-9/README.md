# 27-9 실습: 유효한 pointer arithmetic
이론: [note](../../../notes/27-undefined-behavior/27-9-invalid-pointer-arithmetic.md)
## 실습 목적
같은 배열과 one-past 범위에서만 pointer를 계산한다.
## 작성할 파일
- `main.c`
## 해야 할 일
begin/end pointer로 배열을 순회하고 `ptrdiff_t` 길이를 출력한다.
## 사용할 개념
array object, one-past, pointer subtraction, `ptrdiff_t`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o pointer_arithmetic
```
## 실행 방법
unrelated pointer subtraction은 실행하지 않는다.
```sh
./pointer_arithmetic
```
## 예상 관찰 결과
원소 수 `3`과 합 `15`가 출력된다.
## 확인 포인트
모든 pointer가 같은 array object에서 유도되었는지 확인한다.
## 추가 실습
- ★ subrange를 순회한다.
- ★★ 빈 range를 처리한다.
- ★★★ index loop invariant와 비교한다.
## 완료 기준
one-past를 역참조하거나 범위 밖 pointer를 만들지 않는다.
