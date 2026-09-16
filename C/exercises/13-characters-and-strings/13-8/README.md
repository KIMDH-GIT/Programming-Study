# 13-8 실습: `my_strcpy`

이론: [note](../../../notes/13-characters-and-strings/13-8-my-strcpy.md)
## 실습 목적
characters와 null terminator를 충분한 destination에 복사한다.
## 작성할 파일
`my_strcpy.c`
## 해야 할 일
`"hello"`를 크기 6 destination에 복사하고 출력한다.
## 사용할 개념
source, destination, capacity, element assignment, null terminator.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic my_strcpy.c -o my_strcpy
```
## 실행 방법
```sh
./my_strcpy
```
## 예상 관찰 결과
destination에서 `hello`가 출력된다.
## 확인 포인트
terminator까지 저장하고 capacity 6을 확보했는가?
## 추가 실습
- ★ empty copy - ★★ 한 character - ★★★ capacity 계산
## 완료 기준
- [ ] 경고 없음 - [ ] `hello` 출력 - [ ] 범위 초과 없음
