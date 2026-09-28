# 29-9 실습: `my_strcmp`
이론: [note](../../../notes/29-basic-algorithms/29-9-my-strcmp.md)
## 실습 목적
두 string의 lexicographical ordering sign을 반환한다.
## 작성할 파일
- `main.c`
## 해야 할 일
첫 차이 또는 terminator까지 비교하고 relational result로 `-1/0/1`을 만든다.
## 사용할 개념
C string, unsigned character, prefix, normalized comparison.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strcmp
```
## 실행 방법
```sh
./my_strcmp
```
## 예상 관찰 결과
less, equal, greater cases가 각각 negative, zero, positive를 보인다.

| left | right | 기대 sign | 확인 경계 |
|---|---|---:|---|
| `""` | `""` | 0 | empty |
| `"a"` | `"aa"` | negative | prefix |
| `"same"` | `"same"` | 0 | equal |
| `"z"` | `"a"` | positive | first difference |
## 확인 포인트
arbitrary integer subtraction comparator를 사용하지 않는다.
## 추가 실습
- ★ empty와 nonempty를 비교한다.
- ★★ common prefix가 긴 pair를 검사한다.
- ★★★ standard `strcmp`와 sign만 임시 비교한다.
## 완료 기준
모든 pair에서 expected sign과 종료 위치가 일치한다.
