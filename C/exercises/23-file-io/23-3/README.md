# 23-3 실습: `fprintf`와 `fscanf`
이론: [note](../../../notes/23-file-io/23-3-fprintf-and-fscanf.md)
## 실습 목적
formatted student record를 저장하고 assignment count를 검사해 읽는다.
## 작성할 파일
`formatted_file_io.c`
## 해야 할 일
id와 한 단어 name을 저장한 뒤 `%19s`로 복원한다.
## 사용할 개념
`fprintf`, `fscanf`, format contract, field width.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic formatted_file_io.c -o formatted_file_io
```
## 실행 방법
```sh
./formatted_file_io
```
## 예상 관찰 결과
저장한 id와 name이 출력된다.
## 확인 포인트
format string을 고정하고 return count가 2인지 확인한다.
## 추가 실습
- ★ score를 추가한다.
- ★★ malformed record를 읽는다.
- ★★★ line-based parsing과 비교한다.
## 완료 기준
width와 assignment count를 적용해 warning 없이 round trip한다.
