# 18-18 실습: dynamic array 통계
이론: [note](../../../notes/18-dynamic-memory/18-18-dynamic-array-statistics.md)

## 실습 목적
검증된 입력으로 평균·최댓값·최솟값을 계산한다.
## 작성할 파일
`dynamic_statistics.c`
## 해야 할 일
bounded line과 checked integer conversion으로 양의 count와 정수들을 한 줄씩 입력받는다. 음수·range 초과·trailing junk·allocation overflow·allocation failure를 거부하고 통계를 출력한 뒤 free한다.
## 사용할 개념
input boundary, allocation, average, min, max.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic dynamic_statistics.c -o dynamic_statistics
```
## 실행 방법
```sh
printf '4\n3\n8\n1\n6\n' | ./dynamic_statistics
```
## 예상 관찰 결과
평균 4.50, 최솟값 1, 최댓값 8이 출력된다.
## 확인 포인트
zero·negative·out-of-range count와 element conversion failure path에서도 UB나 leak이 없다.
## 추가 실습
- ★ sum 출력
- ★★ statistics function
- ★★★ line-based input 설계
## 완료 기준
`scanf` 정수 변환에 의존하지 않고 모든 boundary와 cleanup 경로를 처리한다.
