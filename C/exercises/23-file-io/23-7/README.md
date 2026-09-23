# 23-7 실습: text file과 binary file
이론: [note](../../../notes/23-file-io/23-7-text-and-binary-files.md)
## 실습 목적
binary mode로 byte values를 쓰고 portability 경계를 설명한다.
## 작성할 파일
`text_and_binary.c`
## 해야 할 일
세 `unsigned char` values를 `"wb"` stream에 쓰고 count를 검사한다.
## 사용할 개념
text stream, binary stream, translation, object representation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic text_and_binary.c -o text_and_binary
```
## 실행 방법
```sh
./text_and_binary
```
## 예상 관찰 결과
`written elements: 3`이 출력된다.
## 확인 포인트
binary mode를 portable struct serialization이나 disk persistence로 설명하지 않는다.
## 추가 실습
- ★ decimal text로도 저장한다.
- ★★ 현재 platform 결과를 관찰값으로 기록한다.
- ★★★ explicit byte order를 설계한다.
## 완료 기준
text/binary 차이와 representation portability를 설명한다.
