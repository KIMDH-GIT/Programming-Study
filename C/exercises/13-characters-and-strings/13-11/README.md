# 13-11 실습: `strcpy`와 목적지 용량

이론: [note](../../../notes/13-characters-and-strings/13-11-strcpy-capacity.md)
## 실습 목적
destination capacity를 계산한 뒤 library copy를 사용한다.
## 작성할 파일
`strcpy_capacity.c`
## 해야 할 일
`"hello"` source를 크기 6 destination에 `strcpy`로 복사한다.
## 사용할 개념
source string, destination capacity, terminator, `strcpy`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic strcpy_capacity.c -o strcpy_capacity
```
## 실행 방법
```sh
./strcpy_capacity
```
## 예상 관찰 결과
destination에서 `hello`가 출력된다.
## 확인 포인트
capacity가 내용 5 + terminator 1 이상인가?
## 추가 실습
- ★ empty - ★★ larger destination - ★★★ capacity 표
## 완료 기준
- [ ] 경고 없음 - [ ] `hello` 출력 - [ ] capacity 설명
