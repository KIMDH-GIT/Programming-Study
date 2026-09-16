# 11-7 실습: 합과 평균

이론: [note](../../../notes/11-arrays/11-7-sum-average.md)
## 실습 목적
배열 순회로 합과 실수 평균을 계산한다.
## 작성할 파일
`array_sum_average.c`
## 해야 할 일
10, 20, 30, 40, 50의 합과 평균을 출력한다.
## 사용할 개념
array traversal, accumulation, `size_t`, cast, floating division.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_sum_average.c -o array_sum_average
```
## 실행 방법
```sh
./array_sum_average
```
## 예상 관찰 결과
합 150과 평균 30.0이 출력된다.
## 확인 포인트
모든 subscript가 count 미만이고 나눗셈 전에 `double`로 변환하는가?
## 추가 실습
- ★ 세 값 - ★★ 음수 포함 - ★★★ 누적표
## 완료 기준
- [ ] 경고 없음 - [ ] 합 150 - [ ] 평균 30.0
