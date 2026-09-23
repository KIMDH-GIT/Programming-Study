# 23-6 실습: EOF와 읽기 오류
이론: [note](../../../notes/23-file-io/23-6-eof-and-read-errors.md)
## 실습 목적
`fgetc` result와 EOF·error indicators를 올바르게 처리한다.
## 작성할 파일
`eof_and_error.c`
## 해야 할 일
character loop로 file을 읽고 종료 후 `ferror`를 검사한다.
## 사용할 개념
`int` result, `EOF`, `feof`, `ferror`, read-result loop.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic eof_and_error.c -o eof_and_error
```
## 실행 방법
```sh
./eof_and_error
```
## 예상 관찰 결과
file contents와 `EOF reached`가 출력된다.
## 확인 포인트
`char` result와 `while (!feof(fp))`를 사용하지 않는다.
## 추가 실습
- ★ characters를 센다.
- ★★ lines를 센다.
- ★★★ indicators와 `clearerr`를 관찰한다.
## 완료 기준
정상 EOF와 read error 구분을 설명하고 warning 없이 실행한다.
