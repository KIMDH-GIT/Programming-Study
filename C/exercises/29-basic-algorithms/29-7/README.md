# 29-7 실습: `my_strlen`
이론: [note](../../../notes/29-basic-algorithms/29-7-my-strlen.md)
## 실습 목적
terminating null character 전의 길이를 계산한다.
## 작성할 파일
- `main.c`
## 해야 할 일
valid C string만 받고 `size_t` length를 반환한다.
## 사용할 개념
C string, null terminator, `const`, `size_t`, linear scan.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strlen
```
## 실행 방법
```sh
./my_strlen
```
## 예상 관찰 결과
`"C17!!"`은 5, `""`는 0을 출력한다.

| 입력 | 기대 결과 | 확인 경계 |
|---|---:|---|
| `""` | 0 | empty string |
| `"A"` | 1 | one character |
| `"C17!!"` | 5 | normal |
## 확인 포인트
terminator를 결과 length에 포함하지 않고 invalid buffer는 실행하지 않는다.
## 추가 실습
- ★ space 포함 string을 검사한다.
- ★★ standard `strlen`과 임시 비교한다.
- ★★★ pointer notation과 index notation을 비교한다.
## 완료 기준
세 valid string의 deterministic length가 일치한다.
