# 18-8 실습: `free`와 ownership
이론: [note](../../../notes/18-dynamic-memory/18-8-free-and-ownership.md)

## 실습 목적
base pointer를 정확히 한 번 해제한다.
## 작성할 파일
`free_ownership.c`
## 해야 할 일
한 array를 allocation해 사용한 뒤 base pointer를 free하고 NULL을 저장한다. `free(NULL)`도 호출한다.
## 사용할 개념
owner, base pointer, `free(NULL)`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic free_ownership.c -o free_ownership
```
## 실행 방법
```sh
./free_ownership
```
## 예상 관찰 결과
정상 종료하며 별도 출력은 선택이다.
## 확인 포인트
interior pointer와 automatic object 주소를 free하지 않는다.
## 추가 실습
- ★ ownership diagram
- ★★ borrowed pointer contract
- ★★★ alias cleanup analysis
## 완료 기준
allocation base가 한 번만 해제되고 이후 access가 없다.
