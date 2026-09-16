# 13-14 실습: 선택 `my_strcat`

이론: [note](../../../notes/13-characters-and-strings/13-14-my-strcat.md)
## 실습 목적
destination 끝 탐색과 source 복사를 결합한다.
## 작성할 파일
`my_strcat.c`
## 해야 할 일
충분한 destination에 `"C"`와 `" language"`를 연결한다.
## 사용할 개념
destination end, source traversal, terminator, capacity contract.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic my_strcat.c -o my_strcat
```
## 실행 방법
```sh
./my_strcat
```
## 예상 관찰 결과
`C language`가 출력된다.
## 확인 포인트
capacity 11과 마지막 terminator를 보장하는가?
## 추가 실습
- ★ empty source - ★★ empty destination - ★★★ index 표
## 완료 기준
- [ ] 경고 없음 - [ ] 연결 정확 - [ ] 범위 초과 없음
