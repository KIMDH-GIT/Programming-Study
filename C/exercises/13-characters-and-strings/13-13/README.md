# 13-13 실습: `strcat`과 연결 용량

이론: [note](../../../notes/13-characters-and-strings/13-13-strcat-capacity.md)
## 실습 목적
연결 결과와 terminator를 수용하는 destination을 준비한다.
## 작성할 파일
`strcat_capacity.c`
## 해야 할 일
capacity 12인 destination `"hello"`에 `" world"`를 연결한다.
## 사용할 개념
`strcat`, `strlen`, destination capacity, null terminator, non-overlap.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic strcat_capacity.c -o strcat_capacity
```
## 실행 방법
```sh
./strcat_capacity
```
## 예상 관찰 결과
`hello world`가 출력된다.
## 확인 포인트
5 + 6 + 1 elements를 확보했는가?
## 추가 실습
- ★ empty source - ★★ empty destination - ★★★ capacity 표
## 완료 기준
- [ ] 경고 없음 - [ ] 연결 결과 정확 - [ ] capacity 12 설명
