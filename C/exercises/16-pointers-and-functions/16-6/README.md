# 16-6 실습: 배열 매개변수와 pointer adjustment

이론: [note](../../../notes/16-pointers-and-functions/16-6-array-parameter-adjustment.md)
## 실습 목적
actual array와 adjusted pointer parameter를 구분한다.
## 작성할 파일
`array_parameter.c`
## 해야 할 일
3-element array를 `void show_first(int values[])`에 전달해 첫 값을 출력한다.
## 사용할 개념
array object, array expression conversion, parameter adjustment, pass-by-value.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_parameter.c -o array_parameter
```
## 실행 방법
```sh
./array_parameter
```
## 예상 관찰 결과
첫 element 10이 출력된다.
## 확인 포인트
array 전체 복사나 call by reference라고 설명하지 않는가?
## 추가 실습
- ★ sized notation - ★★ pointer notation - ★★★ sizeof comparison
## 완료 기준
- [ ] 경고 없음 - [ ] 10 출력 - [ ] adjustment 설명
