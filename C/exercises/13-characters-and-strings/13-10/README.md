# 13-10 실습: `strlen`과 `sizeof`

이론: [note](../../../notes/13-characters-and-strings/13-10-strlen-sizeof.md)
## 실습 목적
array byte size와 string length를 구분한다.
## 작성할 파일
`strlen_sizeof.c`
## 해야 할 일
empty, `"A"`, `"hello"` arrays의 `sizeof`와 `strlen`을 출력한다.
## 사용할 개념
`sizeof`, `strlen`, `size_t`, `%zu`, null terminator.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic strlen_sizeof.c -o strlen_sizeof
```
## 실행 방법
```sh
./strlen_sizeof
```
## 예상 관찰 결과
각 array에서 `sizeof`가 `strlen + 1`이다.
## 확인 포인트
valid strings에만 `strlen`을 사용하고 `%zu`로 출력하는가?
## 추가 실습
- ★ `"Cat"` - ★★ 중간 null - ★★★ encoding 조사
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 차이 정확 - [ ] terminator 설명
