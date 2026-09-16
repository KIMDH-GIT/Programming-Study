# 11-1 실습: 배열 선언과 초기화

이론: [note](../../../notes/11-arrays/11-1-array-declaration-initialization.md)
## 실습 목적
배열 선언과 initializer의 element 대응을 확인한다.
## 작성할 파일
`array_initialization.c`
## 해야 할 일
5개 `int` 배열을 10, 20, 30, 40, 50으로 초기화하고 index 0, 2, 4를 출력한다.
## 사용할 개념
element type, array identifier, element count, initializer list, index.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_initialization.c -o array_initialization
```
## 실행 방법
```sh
./array_initialization
```
## 예상 관찰 결과
10, 30, 50이 순서대로 출력된다.
## 확인 포인트
5를 마지막 index로 사용하지 않았는가?
## 추가 실습
- ★ `{0}` 초기화 - ★★ 부분 초기화 - ★★★ 크기 생략 초기화
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] 세 값 정확 - [ ] count와 index 구분
