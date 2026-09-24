# 26-4 실습: initialized data와 BSS
이론: [note](../../../notes/26-memory-structure/26-4-initialized-data-and-bss.md)
## 실습 목적
C17 static initialization과 ELF placement 관찰을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
initialized, zero-initialized, const file-scope objects를 출력하고 tools로 관찰한다.
## 사용할 개념
static storage duration, zero initialization, tentative definition, `.data`, BSS, `.rodata`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o data_app
```
## 실행 방법
```sh
./data_app
nm data_app
readelf -S data_app
size data_app
```
## 예상 관찰 결과
`10 0 3`이 출력되고 tool 결과는 current build observation으로 기록한다.
## 확인 포인트
section placement를 C17 guarantee로 바꾸지 않는다.
## 추가 실습
- ★ explicit zero initializer를 추가한다.
- ★★ const와 page protection을 비교한다.
- ★★★ embedded startup을 조사한다.
## 완료 기준
portable value result와 implementation placement를 분리해 설명한다.
