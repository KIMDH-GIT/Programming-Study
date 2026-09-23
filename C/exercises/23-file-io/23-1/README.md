# 23-1 실습: `FILE *`, stream, `fopen`, `fclose`
이론: [note](../../../notes/23-file-io/23-1-file-stream-fopen-fclose.md)
## 실습 목적
학습용 stream을 열고 실패 검사와 명시적 close를 수행한다.
## 작성할 파일
`file_stream.c`
## 해야 할 일
고정 학습용 file을 `"w"`로 열어 한 character를 쓰고 모든 결과를 검사한다.
## 사용할 개념
`FILE`, `FILE *`, `fopen`, null check, `fclose`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic file_stream.c -o file_stream
```
## 실행 방법
```sh
./file_stream
```
## 예상 관찰 결과
`saved`가 출력되고 학습용 file이 생성된다.
## 확인 포인트
`FILE *`를 POSIX file descriptor라고 부르지 않고 closed stream을 재사용하지 않는다.
## 추가 실습
- ★ 저장 character를 바꾼다.
- ★★ open failure를 안전한 path로 관찰한다.
- ★★★ C stream과 OS descriptor를 비교한다.
## 완료 기준
open·write·close 결과를 검사하고 warning 없이 실행한다.
