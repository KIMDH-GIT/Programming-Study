# 26-3 실습: text 영역
이론: [note](../../../notes/26-memory-structure/26-3-text-region.md)
## 실습 목적
function behavior와 ELF symbol·section 관찰을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
function program을 실행하고 `nm`, `readelf -S`, `size`로 현재 build를 관찰한다.
## 사용할 개념
function definition, generated instructions, `.text`, ELF section, symbol.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o text_app
```
## 실행 방법
```sh
./text_app
nm text_app
readelf -S text_app
size text_app
```
## 예상 관찰 결과
program은 `5`를 출력하고 tools는 build-specific symbols·sections를 보여준다.
## 확인 포인트
tool output의 exact address·size·symbol letter를 C17 보장으로 해석하지 않는다.
## 추가 실습
- ★ function 이름을 바꾼다.
- ★★ optimization 영향을 설명한다.
- ★★★ section과 segment를 조사한다.
## 완료 기준
실행 결과는 정확하고 tool 관찰은 implementation-specific으로 설명한다.
