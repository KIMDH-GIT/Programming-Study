# 11-3 실습: 반복문으로 배열 순회

이론: [note](../../../notes/11-arrays/11-3-loop-traversal.md)
## 실습 목적
반복문의 index와 배열 element를 대응한다.
## 작성할 파일
`array_traversal.c`
## 해야 할 일
5개 배열을 `i = 0; i < 5; ++i`로 순회하며 index와 값을 출력한다.
## 사용할 개념
`for`, index, element count, subscript, off-by-one.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_traversal.c -o array_traversal
```
## 실행 방법
```sh
./array_traversal
```
## 예상 관찰 결과
index 0~4와 대응 값이 다섯 줄 출력된다.
## 확인 포인트
index 5를 접근하지 않는가?
## 추가 실습
- ★ 1 더해 출력 - ★★ 짝수만 출력 - ★★★ 추적표
## 완료 기준
- [ ] 경고 없음 - [ ] 다섯 element 출력 - [ ] 경계 정확
