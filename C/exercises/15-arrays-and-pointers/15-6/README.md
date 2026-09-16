# 15-6 실습: 같은 배열과 one-past 경계

이론: [note](../../../notes/15-arrays-and-pointers/15-6-one-past-boundary.md)
## 실습 목적
one-past pointer를 dereference 없이 endpoint로 사용한다.
## 작성할 파일
`one_past_boundary.c`
## 해야 할 일
begin/end pointers로 다섯 elements를 출력하고 end 도달을 확인한다.
## 사용할 개념
same array, one-past, relational comparison, valid dereference.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic one_past_boundary.c -o one_past_boundary
```
## 실행 방법
```sh
./one_past_boundary
```
## 예상 관찰 결과
다섯 values와 end 도달 확인이 출력된다.
## 확인 포인트
end pointer를 절대 dereference하지 않는가?
## 추가 실습
- ★ condition 분석 - ★★ count 1 - ★★★ 위치 분류
## 완료 기준
- [ ] 경고 없음 - [ ] 다섯 values - [ ] one-past 안전
